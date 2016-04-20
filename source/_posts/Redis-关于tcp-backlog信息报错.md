---
title: Redis 关于tcp-backlog信息报错
date: 2016-04-18 18:58:08
categories: 数据库
tags: [Redis, NoSQL]
---

```bash
Starting Redis server...
  
*** FATAL CONFIG FILE ERROR ***
Reading the configuration file, at line 54
>>> 'tcp-backlog 511'
Bad directive or wrong number of arguments
```


<!--more-->


```bash
[root@localhost redis]# redis-server -v
Redis server version 2.4.10 (00000000:0)
```
--------------------------------------解决方案--------------------------------------------
你所使用的redis版本不支持该配置参数。需要将版本改为redis2.6或者 2.8版本的较低二进制，亦或者提供更好的配置文件。

究其原因是因为版本太低的缘故。