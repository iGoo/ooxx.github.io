---
title: crontab中百分号(%)需要转义
s: Crontab percent (%) need to escape
date: 2016-04-20 23:09:23
tags: crontab
categories: 默认分类
---

![请输入图片描述][1]
需要写个定时任务，每分钟把时间输出到`/root/time.log`文件里面
因为需要部署到几十台服务器上，写个脚本再写定时任务的话很麻烦，所以想直接在crontab里面写命令输出


<!--more-->


```bash
* * * * * date +”%F %H:%M” >> /root/time.log 2>&1
```
部署上去之后，发现并没有生效
查看日志`/var/log/cron`
`Mar  6 15:06:01 localhost CROND[2188]: (root) CMD (date +")`
并没有执行
这是因为`%`在crontab里面是有特殊的意义的，需要使用`\`进行转义
```bash
* * * * * /bin/date +"\%F \%H:\%M" >> /root/time.log 2>&1
```
这样子就ok了


[1]: http://static.keer.me/o_1ae3vl40k1orm6focvd15kl96a.png

