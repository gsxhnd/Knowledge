
## 查看网卡名称

```shell
ip link show
```

##### 假设网卡名称为eth0 (注意 有些设备的网卡名称为end0 要根据上述命令查看的)

###### 假设你的主路由器网关是192.168.66.1

##### Docker 中创建一个 macvlan 网络

```shell
docker network create -d macvlan \
  --subnet=192.168.66.0/24 \
  --gateway=192.168.66.1 \
  -o parent=eth0 \
  macnet
```

不用开启混杂模式 因为 `macvlan` 网络与 `docker0` 的桥接网络完全不同，它不依赖 `docker0` ，而是直接与宿主机的物理接口

~~==ip link set docker0 promisc on==~~

##### 打印docker中的macvlan网络是否创建成功

```shell
docker network ls
```

macvlan 的一个特性是宿主机无法直接与容器通信。如果你的需求是让宿主机与 OpenWrt 容器通信，你需要在 **宿主机上创建一个虚拟接口** （通常称为 macvlan 子接口），并将其加入同一 macvlan 网络。

```shell
ip link add macvlan-shim link eth0 type macvlan mode bridge
ip addr add 192.168.66.2/24 dev macvlan-shim
ip link set macvlan-shim up
```

注意检查上述ip地址 `192.168.66.2` 确保它没有被其他设备占用

- `macvlan-shim` 是虚拟接口的名称，你可以自定义。
- `192.168.66.2/24` 是给宿主机虚拟接口分配的 IP 地址，应位于 `192.168.66.0/24` 子网内，且不冲突。

**添加路由（如果需要）** 如果宿主机需要通过 macvlan 网络访问容器，可以添加路由：

```shell
ip route add 192.168.66.0/24 dev macvlan-shim
```

`dev` 是 **device** 的缩写，用来指定路由条目所绑定的网络接口（设备）。

## 运行我们自己创建的docker 容器

```shell
docker run --name immortalwrt -d --network macnet --privileged immortalwrt-image:latest /sbin/init
```

`immortalwrt` 为docker容器名称

`immortalwrt-image` 是docker镜像名称（上述docker build 所得）

————————————————————————\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*————————————————————

## 运行后 ，进入容器，容器内就是immortalwrt 系统啦

```shell
docker exec -it immortalwrt sh
```

## 在ImmortalWrt 命令行里设置静态ip（vim编辑器）

```shell
vi /etc/config/network
```

```shell
config interface 'lan'
        option ifname 'eth0'
        option proto 'static'
        option netmask '255.255.255.0'
        option ipbassign '60'
        option ipaddr '192.168.66.88'
        option gateway '192.168.66.1'
        option dns '223.5.5.5 1.1.1.1'
```

## 为了避免大家使用vim编辑器，你可以复制如下代码到命令

```shell
cat <<EOF > /etc/config/network
config interface 'loopback'
    option device 'lo'
    option proto 'static'
    option ipaddr '127.0.0.1'
    option netmask '255.0.0.0'

config globals 'globals'
    option ula_prefix 'fd98:9655:39f9::/48'

config interface 'lan'
    option proto 'static'
    option netmask '255.255.255.0'
    option ipbassign '60'
    option ipaddr '192.168.66.88'
    option gateway '192.168.66.1'
    option dns '223.5.5.5 1.1.1.1'
    option device 'eth0'
EOF
```

上述代码中 `192.168.66.88` 是我设置的ip地址，你要 **根据自己主路由器的ip网段** 来调整。

## 重启ImmortalWrt的网络

```shell
/etc/init.d/network restart
```

## 如果imm没有网 就在宿主机再次执行一次

```shell
ip link set macvlan-shim up
```

`luci-i18n-filebrowser-go-zh-cn`

## 后记

## 在docker 版的immortalwrt中安装一些必备插件

```shell
opkg update
opkg install luci-i18n-ttyd-zh-cn
opkg install luci-i18n-filebrowser-go-zh-cn
opkg install luci-i18n-argon-config-zh-cn
opkg install openssh-sftp-server
opkg install luci-i18n-samba4-zh-cn
```

## 安装iStore商店(ARM64 & x86-64通用)

```shell
wget -qO imm.sh https://cafe.cpolar.top/wkdaily/zero3/raw/branch/main/zero3/imm.sh && chmod +x imm.sh && ./imm.sh
```

## 安装网络向导和首页(ARM64 & x86-64通用)

```shell
is-opkg install luci-i18n-quickstart-zh-cn
```

## 为什么制作docker版本的immortalwrt?

OpenWrt 也分很多圈子，他们各自为营。相互之前，不会有太多交流。但一定会有很多纷争，毫不夸张的说，有点像饭圈文化。

没什么道理可言，只要意见相左 就开喷。

我做的教程 更多是服务于我的观众。

那些不分青红皂白，在背后骂我的人 肯定是有的，比如有的人是搜索引擎搜到我的，并非我的观众。但只要不在我的视线范围，我就当作没看到。人不犯我 我不犯人。再者说 我在公共流量范围内，而你在暗处，这太不公平了。我的一言一行都被社交媒体放大一千倍。我犯不上跟你置气～

docker版本的场景无非就2种，1、NAS中，有人想做个OpenWrt的旁路由 2、很多单网口电视盒子由于openwrt不支持，所以制作docker版本。最终也是用来做旁路由。我的想法是相对开放的 虽然我自己还是建议用普通的双网口路由模式，但我不抵制 也不反对别人用旁路由。我觉得用的人 只要能解决自己的问题就行了。那种莫名其妙的优越感 携带着一些污言秽语的人。请自觉离开。什么是建议、什么是侮辱 我还是分得清的。
