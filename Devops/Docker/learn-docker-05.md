---
title: Docker daemon.json 文件
created: 2024-05-09 09:58
---

<!-- markdownlint-disable MD025 -->

# Docker daemon.json 文件

docker 安装后默认没有 daemon.json 这个配置文件，需要进行手动创建。配置文件的默认路径：/etc/docker/daemon.json

一般情况，配置文件 daemon.json 中配置的项目参数，在启动参数中同样适用，有些可能不一样（具体可以查看官方文档），但需要注意的一点，配置文件中如果已经有某个配置项，则无法在启动参数中增加，会出现冲突的错误。

如果在 daemon.json 文件中进行配置，需要 docker 版本高于 1.12.6(在这个版本上不生效，1.13.1 以上是生效的)

参数-
daemon.json 文件可配置的参数表，我们在配置的过程中，只需要设置我们需要的参数即可，不必全部写出来。详细参考官网。

官方的配置地址：<https://docs.docker.com/engine/reference/commandline/dockerd/#/configuration-reloading>

官方的配置地址：<https://docs.docker.com/engine/reference/commandline/dockerd/#options>

官方的配置地址：<https://docs.docker.com/engine/reference/commandline/dockerd/#/linux-configuration-file>

```json
{
  "api-cors-header": "",
  "authorization-plugins": [

  ],
  "bip": "",
  "bridge": "",
  "cgroup-parent": "",
  "cluster-store": "",
  "cluster-store-opts": {

  },
  "cluster-advertise": "",
  "debug": true,
  #启用debug的模式，启用后，可以看到很多的启动信息。默认false"default-gateway": "",
  "default-gateway-v6": "",
  "default-runtime": "runc",
  "default-ulimits": {

  },
  "disable-legacy-registry": false,
  "dns": [
    "192.168.1.1"
  ],
  #设定容器DNS的地址，在容器的/etc/resolv.conf文件中可查看。"dns-opts": [

  ],
  #容器/etc/resolv.conf文件，其他设置"dns-search": [

  ],
  #设定容器的搜索域，当设定搜索域为.example.com时，在搜索一个名为host的主机时，DNS不仅搜索host，还会搜索host.example.com。注意：如果不设置，Docker会默认用主机上的/etc/resolv.conf来配置容器。"exec-opts": [

  ],
  "exec-root": "",
  "fixed-cidr": "",
  "fixed-cidr-v6": "",
  "graph": "/var/lib/docker",
  ＃已废弃，使用data-root代替,
  这个主要看docker的版本"data-root": "/var/lib/docker",
  ＃Docker运行时使用的根路径,
  根路径下的内容稍后介绍，默认/var/lib/docker"group": "",
  #Unix套接字的属组,
  仅指/var/run/docker.sock"hosts": [

  ],
  #设置容器hosts"icc": false,
  "insecure-registries": [

  ],
  #配置docker的私库地址"ip": "0.0.0.0",
  "iptables": false,
  "ipv6": false,
  "ip-forward": false,
  #默认true,
  启用net.ipv4.ip_forward,
  进入容器后使用sysctl-a|grepnet.ipv4.ip_forward查看"ip-masq": false,
  "labels": [
    "nodeName=node-121"
  ],
  #docker主机的标签，很实用的功能,
  例如定义：–labelnodeName=host-121"live-restore": true,
  "log-driver": "",
  "log-level": "",
  "log-opts": {

  },
  "max-concurrent-downloads": 3,
  "max-concurrent-uploads": 5,
  "mtu": 0,
  "oom-score-adjust": -500,
  "pidfile": "",
  #Docker守护进程的PID文件"raw-logs": false,
  "registry-mirrors": [
    "xxxx"
  ],
  #镜像加速的地址，增加后在dockerinfo中可查看。"runtimes": {
    "runc": {
      "path": "runc"
    },
    "custom": {
      "path": "/usr/local/bin/my-runc-replacement",
      "runtimeArgs": [
        "--debug"
      ]
    }
  },
  "selinux-enabled": false,
  #默认false，启用selinux支持"storage-driver": "",
  "storage-opts": [

  ],
  "swarm-default-advertise-addr": "",
  "tls": true,
  #默认false,
  启动TLS认证开关"tlscacert": "",
  #默认~/.docker/ca.pem，通过CA认证过的的certificate文件路径"tlscert": "",
  #默认~/.docker/cert.pem，TLS的certificate文件路径"tlskey": "",
  #默认~/.docker/key.pem，TLS的key文件路径"tlsverify": true,
  #默认false，使用TLS并做后台进程与客户端通讯的验证"userland-proxy": false,
  "userns-remap": ""
}
```

上述是官网 docs 提供的一个示例配置，我们可以参考，选择性的配置其中的部分内容。

## 示例

1、如何配置 registry 私库相关的参数-
涉及以下 2 个参数：

```text
"insecure-registries": [], #这个私库的服务地址
"registry-mirrors": [], #私库加速器
```

2.配置示例：

```shell
$ cat /etc/docker/daemon.json

{
  "registry-mirrors": [
    "https://d8b3zdiw.mirror.aliyuncs.com"
  ],
  "insecure-registries": [
    "https://ower.site.com"
  ]
}
```

### 配置与应用

1. 默认没有文件，所以我们需要先创建，进入/etc/docker 目录下，记得创建的文件所有者是 root（vim 或 touch，记得 chown 修改所有者）

   `-rw-r--r-- 1 root root 71 Dec 19 17:25daemon.json`

2. 在文档中配置想要添加的参数：如，镜像加速器网站,私库网站

```shell
$ cat /etc/docker/daemon.json

{
    "registry-mirrors":
        [ "https://d8b3zdiw.mirror.aliyuncs.com"],
    "insecure-registries":
        [ "https://ower.site.com" ]
}
```

3. 创建并修改完 daemon.json 文件后，需要让这个文件生效-
   a. 修改完成后 reload 配置文件

   > sudo systemctl daemon-reload

   b. 重启 docker 服务

   > sudo systemctl restart docker.service

   c. 查看状态

   > sudo systemctl status docker -l

   d. 查看服务

   > sudo docker info

当我们需要对 docker 服务进行调整配置时，不用去修改主文件 docker.service 的参数，通过 daemon.json 配置文件来管理，更为安全、合理。
