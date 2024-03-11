---
title: OpenWRT 安装
creation date: 2023-03-23 20:48
---


<!-- markdownlint-disable MD025 -->

# Untitled

`sed -ibak 's/https://downloads.openwrt.org/https://downloads.openwrt.org//g' /etc/opkg/distfeeds.conf`

`opkg install luci luci-base luci-compat dnsmasq-full`
`opkg install fdisk smartmontools smartmontools-drivedb`

```
# /dev/nvme0n1p3 xiugai
# https://www.moewah.com/archives/4719.html
mkdir -p /tmp/introot
mkdir -p /tmp/extroot
mount --bind / /tmp/introot
mount /dev/nvme0n1p3 /tmp/extroot
tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
umount /tmp/introot
umount /tmp/extroot
```


`opkg install smartdns luci-app-smartdns luci-i18n-smartdns-zh-cn`
`opkg install wrtbwmon_1.2.1-3_all.ipk luci-app-wrtbwmon_2.0.10_all.ipk luci-i18n-wrtbwmon-zh-cn_211213.07899_all.ipk`
`opkg install docker docker-compose dockerd luci-app-dockerman luci-i18n-dockerman-zh-cn`

```
opkg install coreutils-nohup bash iptables dnsmasq-full curl ca-certificates ipset ip-full iptables-mod-tproxy iptables-mod-extra libcap libcap-bin ruby ruby-yaml kmod-tun kmod-inet-diag unzip luci-compat luci luci-base
```