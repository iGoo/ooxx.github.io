---
title: Linux命令使用小技巧
s: linux-command-tips
date: 2016-04-22 19:09:12
tags: [Linux, Command]
categories: Linux
---
`mkdir` 加`-p`参数的时候递归闯进简写
`mkdir -p /usr/local/redis/{etc,bin,share/man}` 这样可以创建一个类似下面的目录树结构
```
.
├── bin
├── etc
└── share
    └── man
```
<!-- more -->
**查看命令依赖共享库**
`ldd /bin/su` `当某个命令出现问题就查询此命令所依赖的库文件是否出现了权限等问题`

**查看某个目录的详细权限**
```sh
[root@AY conf.d]# stat /
    File: `/'
    Size: 4096          Blocks: 8          IO Block: 4096   directory
    Device: ca01h/51713d    Inode: 2           Links: 24
    Access: (0555/dr-xr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
    Access: 2015-11-12 12:14:14.571167538 +0800
    Modify: 2015-06-16 14:17:14.151684417 +0800
    Change: 2015-06-16 14:17:14.151684417 +0800
```

反引号`` ` ``小技巧
```sh
    [vagrant@vagrant-centos65 ~]$ echo kernel-devel-`uname -r`
    kernel-devel-2.6.32-431.3.1.el6.x86_64
```