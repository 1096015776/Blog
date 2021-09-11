---
title: Arch Linux And Dwm
date: 2021-07-15 17:36:49
tags:
  - Install
  - Dwm
categories:
  - Linux
---

## why

1. less is more , 去掉无用的东西
2. 保持代码整洁
3. 喜欢 linux 并开始尝试
4. linux 让我开始明白 help 文档

## prepare

1. 安装的所有问题 都可以在 archwiki 中找到(记录主要的步骤)
2. 至少你有一个系统，以防安装失败，无法使用电脑
3. 建议参照 archwiki 安装

## start

1. 将终端字体更改为自己舒适的字体

```bash
setfont /usr/share
```

2. 连上网络，以确保安装最新的包

```bash
#给电脑动态分配ip地址
dhcpcd&
#查看网卡
ip link
#开启网卡
ip link set wlp3s0 up
#扫描wifi
iwlist wlp3s0 scan
#wpa_passphrase 生成连接文件
wpa_passphrase essid passphrase > internet.conf
#wpa_supplicant 连接wifi
wpa_supplicant -i wlp3s0 -c internet.conf
```

3. 分区准备格式化为安装系统做准备

```bash
#fdisk 命令
#mkfs.fat mkfs.ext4 mkfs.swap 格式化分区 swapon
```
