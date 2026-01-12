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

## Refer

- [如何制作docker版Immortalwrt](https://wkdaily.cpolar.cn/archives/15)
