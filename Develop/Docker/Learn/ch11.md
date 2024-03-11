# 11. Docker 网络模式

Docker 安装时会自动在 host 上创建三个网络，我们可用 `docker network ls` 命令查看：

```shell
$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
174dbe6f8d20   bridge            bridge    local
6af2b3804c44   host              host      local
1e68acb2e18c   mariadb_default   bridge    local
cb3a14cc8ae2   none              null      local
```

- none 模式，使用--net=none 指定，该模式关闭了容器的网络功能。
- host 模式，使用--net=host 指定，容器将不会虚拟出自己的网卡，配置自己的 IP 等，而是使用宿主机的 IP 和端口。
- bridge 模式，使用--net=bridge 指定，默认设置 ，此模式会为每一个容器分配、设置 IP 等，并将容器连接到一个 docker0 虚拟网桥，通过 docker0 网桥以及 Iptables nat 表配置与宿主机通信。
- container 模式，使用--net=container:NAME_or_ID 指定，创建的容器不会创建自己的网卡，配置自己的 IP，而是和一个指定的容器共享 IP、端口范围。

## none 网络

只能访问本地网络，没有外网。挂在这个网络下的容器除了 lo，没有其他任何网卡。容器创建时，可以通过 `--network=none` 指定使用 none 网络。

## host 网络

连接到 host 网络的容器共享 Docker host 的网络栈，容器的网络配置与 host 完全一样。可以通过 `--network=host` 指定使用 host 网络。

直接使用 Docker host 的网络最大的好处就是性能，如果容器对网络传输效率有较高要求，则可以选择 host 网络。当然不便之处就是牺牲一些灵活性，比如要考虑端口冲突问题，Docker host 上已经使用的端口就不能再用了。

Docker host 的另一个用途是让容器可以直接配置 host 网路。比如某些跨 host 的网络解决方案，其本身也是以容器方式运行的，这些方案需要对网络进行配置，比如管理 iptables。

## bridge 网络

Docker 安装时会创建一个 命名为 `docker0` 的 linux bridge。如果不指定`--network`，创建的容器默认都会挂到 `docker0` 上

brctl show #查看 bridge 网络 yum install bridge-utils

docker network inspect bridge #查看 bridge 网络的详细信息

```shell
[
    {
        "Name": "bridge",
        "Id": "174dbe6f8d203e6bcf09a39c8509dc672597825b8dfdfc27de6a3a87cf7e083c",
        "Created": "2021-11-04T06:14:32.699022433Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

当前 docker0 上没有任何其他网络设备，我们创建一个容器看看有什么变化。

一个新的网络接口 `veth28c57df` 被挂到了 `docker0` 上，`veth28c57df`就是新创建容器的虚拟网卡。

下面看一下容器的网络配置。

容器有一个网卡 `eth0@if34`。大家可能会问了，为什么不是`veth28c57df` 呢？

实际上 `eth0@if34` 和 `veth28c57df` 是一对 veth pair。veth pair 是一种成对出现的特殊网络设备，可以把它们想象成由一根虚拟网线连接起来的一对网卡，网卡的一头（`eth0@if34`）在容器中，另一头（`veth28c57df`）挂在网桥 `docker0` 上，其效果就是将 `eth0@if34` 也挂在了 `docker0` 上。

我们还看到 `eth0@if34` 已经配置了 IP `172.17.0.2`，为什么是这个网段呢？让我们通过 `docker network inspect bridge` 看一下 bridge 网络的配置信息:

原来 bridge 网络配置的 subnet 就是 172.17.0.0/16，并且网关是 172.17.0.1。这个网关在哪儿呢？大概你已经猜出来了，就是 docker0。

当前容器网络拓扑结构如图所示：

容器创建时，docker 会自动从 172.17.0.0/16 中分配一个 IP，这里 16 位的掩码保证有足够多的 IP 可以供容器使用。

## other container 网络

$ docker run -it --name feiyu-con --net=container:feiyu busybox sh

除了 none, host, bridge 这三个自动创建的网络，用户也可以根据业务需要创建 user-defined 网络。

### user-defined 网络

**Docker 提供三种 user-defined 网络驱动：bridge, overlay 和 macvlan。overlay 和 macvlan 用于创建跨主机的网络。**

我们可通过 bridge 驱动创建类似前面默认的 bridge 网络，例如：

查看一下当前 host 的网络结构变化：

新增了一个网桥 `br-eaed97dc9a77`，这里 `eaed97dc9a77` 正好新建 bridge 网络 `my_net` 的短 id。执行 `docker network inspect` 查看一下 `my_net` 的配置信息：

这里 172.18.0.0/16 是 Docker 自动分配的 IP 网段。

我们可以自己指定 IP 网段吗？-
答案是：可以。

只需在创建网段时指定 `--subnet` 和 `--gateway` 参数：

这里我们创建了新的 bridge 网络 `my_net2`，网段为 172.22.16.0/24，网关为 172.22.16.1。与前面一样，网关在 `my_net2` 对应的网桥 `br-5d863e9f78b6` 上：

容器要使用新的网络，需要在启动时通过 `--network` 指定：

容器分配到的 IP 为 172.22.16.2。

到目前为止，容器的 IP 都是 docker 自动从 subnet 中分配，我们能否指定一个静态 IP 呢？

答案是：可以，通过`--ip`指定。

注：**只有使用 `--subnet` 创建的网络才能指定静态 IP**。

`my_net` 创建时没有指定 `--subnet`，如果指定静态 IP 报错如下：

好了，我们来看看当前 docker host 的网络拓扑结构。

通过前面小节的实践，当前 docker host 的网络拓扑结构如下图所示，今天我们将讨论这几个容器之间的连通性。

两个 busybox 容器都挂在 my_net2 上，应该能够互通，我们验证一下：

可见同一网络中的容器、网关之间都是可以通信的。

`my_net2` 与默认 bridge 网络能通信吗？

从拓扑图可知，两个网络属于不同的网桥，应该不能通信，我们通过实验验证一下，让 busybox 容器 ping httpd 容器：

确实 ping 不通，符合预期。

“等等！不同的网络如果加上路由应该就可以通信了吧？”我已经听到有读者在建议了。

这是一个非常非常好的想法。

确实，如果 host 上对每个网络的都有一条路由，同时操作系统上打开了 ip forwarding，host 就成了一个路由器，挂接在不同网桥上的网络就能够相互通信。下面我们来看看 docker host 满不满足这些条件呢？

`ip r` 查看 host 上的路由表：

\# ip r

......

172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1

172.22.16.0/24 dev br-5d863e9f78b6 proto kernel scope link src 172.22.16.1

......

172.17.0.0/16 和 172.22.16.0/24 两个网络的路由都定义好了。再看看 ip forwarding：

\# sysctl net.ipv4.ip_forward

net.ipv4.ip_forward = 1

ip forwarding 也已经启用了。

条件都满足，为什么不能通行呢？

我们还得看看 iptables：

\# iptables-save

......

\-A DOCKER-ISOLATION -i br-5d863e9f78b6 -o docker0 -j DROP

\-A DOCKER-ISOLATION -i docker0 -o br-5d863e9f78b6 -j DROP

......

原因就在这里了：**iptables DROP 掉了网桥 docker0 与 br-5d863e9f78b6 之间双向的流量**。

从规则的命名 `DOCKER-ISOLATION` 可知 docker 在设计上就是要隔离不同的 netwrok。

那么接下来的问题是：怎样才能让 busybox 与 httpd 通信呢？

答案是：为 httpd 容器添加一块 net_my2 的网卡。这个可以通过`docker network connect` 命令实现。

我们在 httpd 容器中查看一下网络配置：

容器中增加了一个网卡 eth1，分配了 my_net2 的 IP 172.22.16.3。现在 busybox 应该能够访问 httpd 了，验证一下：

busybox 能够 ping 到 httpd，并且可以访问 httpd 的 web 服务。当前网络结构如图所示：

**容器之间可通过 IP，Docker DNS Server 或 joined 容器三种方式通信**

#### IP 通信

从上一节的例子可以得出这样一个结论：两个容器要能通信，必须要有属于同一个网络的网卡。

满足这个条件后，容器就可以通过 IP 交互了。具体做法是在容器创建时通过 `--network` 指定相应的网络，或者通过 `docker network connect` 将现有容器加入到指定网络。可参考上一节 httpd 和 busybox 的例子，这里不再赘述。

#### Docker DNS Server

通过 IP 访问容器虽然满足了通信的需求，但还是不够灵活。因为我们在部署应用之前可能无法确定 IP，部署之后再指定要访问的 IP 会比较麻烦。对于这个问题，可以通过 docker 自带的 DNS 服务解决。

从 Docker 1.10 版本开始，docker daemon 实现了一个内嵌的 DNS server，使容器可以直接通过“容器名”通信。方法很简单，只要在启动时用 `--name` 为容器命名就可以了。

下面启动两个容器 bbox1 和 bbox2：

docker run -it --network=my_net2 --name=bbox1 busybox

docker run -it --network=my_net2 --name=bbox2 busybox

然后，bbox2 就可以直接 ping 到 bbox1 了：

使用 docker DNS 有个限制：**只能在 user-defined 网络中使用**。也就是说，默认的 bridge 网络是无法使用 DNS 的。下面验证一下：

创建 bbox3 和 bbox4，均连接到 bridge 网络。

docker run -it --name=bbox3 busybox

docker run -it --name=bbox4 busybox

bbox4 无法 ping 到 bbox3。

#### joined 容器

joined 容器是另一种实现容器间通信的方式。

joined 容器非常特别，它可以使两个或多个容器共享一个网络栈，共享网卡和配置信息，joined 容器之间可以通过 127.0.0.1 直接通信。请看下面的例子：

先创建一个 httpd 容器，名字为 web1。

docker run -d -it --name=web1 httpd 然后创建 busybox 容器并通过 `--network=container:web1` 指定 jointed 容器为 web1：

请注意 busybox 容器中的网络配置信息，下面我们查看一下 web1 的网络：

看！busybox 和 web1 的网卡 mac 地址与 IP 完全一样，它们共享了相同的网络栈。busybox 可以直接用 127.0.0.1 访问 web1 的 http 服务。

joined 容器非常适合以下场景：

1.  不同容器中的程序希望通过 loopback 高效快速地通信，比如 web server 与 app server。

2.  希望监控其他容器的网络流量，比如运行在独立容器中的网络监控程序。
