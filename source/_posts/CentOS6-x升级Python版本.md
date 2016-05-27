---
title: CentOS6.x升级Python版本
date: 2016-05-27 17:28:38
tags: [CentOS, Python]
categories: Linux
---

> 较低版本的Python会在使用`pip`安装软件时候提示版本太低

1. 首先下载你要安装的Python版本源码包，这里我安装的是2.7.10

```sh
    wget -c https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tgz
    tar zxvf Python-2.7.10
    cd Python-2.7.10.tgz
    ./configure --prefix=/usr/local/python27
    make && make install
    ln -s /usr/local/python27/bin/python /usr/bin/python
    # python -V
    Python 2.7.10
```
<!-- more -->

2. 修复yum

```sh
    vim `which yum`
    #修改第一行为 #!/usr/bin/python2.6 
```

3. 安装pip

```sh
    wget -c https://bootstrap.pypa.io/get-pip.py
    python get-pip.py
```