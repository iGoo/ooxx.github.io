---
title: 'Alpine轻量安全的linux发行版 '
s: try-alpine-linux-release
date: 2016-07-25 18:57:24
tags: [alpine]
categories: Linux
---

> Alpine Linux 是一个面向安全应用的轻量级 Linux 发行版。它采用了musl libc和busybox以减小系统的体积和运行时资源消耗，同时还提供了自己的包管理工具apk。Alpine Linux的内核都打了grsecurity/PaX补丁，并且所有的程序都编译为Position Independent Executables (PIE) 以增强系统的安全性。

看到`Docker`官方已经开始使用`Alpine`来替换`Ubuntu`系统作为官方的系统镜像。然后发现`Alpine`真的很小，小到`Docker`官方的Alpine镜像只有5M左右。



```sh
echo "http://nl.alpinelinux.org/alpine/v3.3/community" >> /etc/apk/repositories
apk update
apk add docker
rc-update add docker boot
service docker start
```