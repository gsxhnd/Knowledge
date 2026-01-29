---
title: Docker 构建 ImmortalWrt 镜像
created: 2026-01-12 14:54:42
published: 2026-01-12 14:54:42
tags:
  - Docker
  - ImmortalWrt
---
<!-- markdownlint-disable MD025 -->

# Docker 构建 ImmortalWrt 镜像

## 制作镜像

### 下载 rootfs

<https://downloads.immortalwrt.org/releases/>

- [x86-64](https://downloads.immortalwrt.org/releases/24.10.3/targets/x86/64/immortalwrt-24.10.3-x86-64-rootfs.tar.gz)
- [arm-64](https://downloads.immortalwrt.org/releases/24.10.3/targets/armsr/armv8/immortalwrt-24.10.3-armsr-armv8-rootfs.tar.gz)
- [arm-32](https://downloads.immortalwrt.org/releases/24.10.3/targets/armsr/armv7/immortalwrt-24.10.3-armsr-armv7-rootfs.tar.gz)

```shell
mkdir wrt
cd wrt
wget -O rootfs.tar.gz https://downloads.immortalwrt.org/releases/24.10.3/targets/armsr/armv8/immortalwrt-24.10.3-armsr-armv8-rootfs.tar.gz
gzip -d rootfs.tar.gz
```

### 创建一个Dockerfile文件

```Dockerfile
FROM scratch
ADD rootfs.tar /
```

### 执行构建

```shell
docker build -t immortalwrt .
```

## 创建网络

```shell
# 查看网络
ip link show
```

不用开启混杂模式 因为 `macvlan` 网络与 `docker0` 的桥接网络完全不同，它不依赖 `docker0` ，而是直接与宿主机的物理接口

```shell
# 假设网卡名称为eth0
# 假设你的主路由器网关是192.168.66.1
docker network create -d macvlan \
  --subnet=192.168.66.0/24 \
  --gateway=192.168.66.1 \
  -o parent=eth0 \
  macnet

# 查看docker中的macvlan网络是否创建成功
docker network ls
```

macvlan 的一个特性是宿主机无法直接与容器通信。如果你的需求是让宿主机与 OpenWrt 容器通信，你需要在 **宿主机上创建一个虚拟接口** （通常称为 macvlan 子接口），并将其加入同一 macvlan 网络。

```shell
ip link add macvlan-shim link eth0 type macvlan mode bridge
ip addr add 192.168.66.2/24 dev macvlan-shim
ip link set macvlan-shim up
```

> 注意检查上述ip地址 `192.168.66.2` 确保它没有被其他设备占用

- `macvlan-shim` 是虚拟接口的名称，你可以自定义。
- `192.168.66.2/24` 是给宿主机虚拟接口分配的 IP 地址，应位于 `192.168.66.0/24` 子网内，且不冲突。
- `dev` 是 **device** 的缩写，用来指定路由条目所绑定的网络接口（设备）。

**添加路由（如果需要）** 如果宿主机需要通过 macvlan 网络访问容器，可以添加路由：

```shell
ip route add 192.168.66.0/24 dev macvlan-shim
```

## 运行容器

```shell
docker run --name immortalwrt -d --network macnet --privileged immortalwrt-image:latest /sbin/init
```

- `immortalwrt` 为docker容器名称
- `immortalwrt-image` 是docker镜像名称（上述docker build 所得）

### 在ImmortalWrt 命令行里设置静态ip

```shell
# 运行后 ，进入容器，容器内就是 immortalwrt 系统
docker exec -it immortalwrt sh
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

### 为了避免大家使用vim编辑器，你可以复制如下代码到命令

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

### 重启ImmortalWrt的网络

```shell
/etc/init.d/network restart
```

```shell
# 如果imm没有网 就在宿主机再次执行一次
ip link set macvlan-shim up
```

## 安装依赖

在docker 版的immortalwrt中安装一些必备插件

```shell
opkg update
opkg install luci-i18n-ttyd-zh-cn
opkg install luci-i18n-filebrowser-go-zh-cn
opkg install luci-i18n-argon-config-zh-cn
opkg install openssh-sftp-server
# opkg install luci-i18n-samba4-zh-cn

# 安装网络向导和首页(ARM64 & x86-64通用)
is-opkg install luci-i18n-quickstart-zh-cn
```

## Refer

- [如何制作docker版Immortalwrt](https://wkdaily.cpolar.cn/archives/15)
