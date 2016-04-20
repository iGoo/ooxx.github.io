---
title: 如何为Let’s Encrypt颁发的SSL证书续期
s: How to renew the Let certificate issued by s' Encrypt SSL
date: 2016-04-20 23:14:30
tags: let's encrypt
---

![Lets-Encrypt-1024x410.png][1]
<!--more-->

1.停止web服务器
`sudo service nginx stop     or      sudo systemctl stop nginx`

2.输入letsencrypt客户端命令进行续期操作
```bash
cd /letsencrypt

./letsencrypt-auto renew --email your-email-address --agree-tos
```
如果续期成功将会有下面的类似提示
```bash
Processing /etc/letsencrypt/renewal/keer.me.conf
new certificate deployed without reload, fullchain is /etc/letsencrypt/live/keer.me/fullchain.pem

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/keer.me/fullchain.pem (success)
```

3.续期后重启Web Server
```bash
sudo service nginx start        or        sudo systemctl start nginx
```


> 如果你的网站受CDN的防护，那么在续期之前，你需要修改域名的A记录，将www域名和不带www的域名的A记录都指向你的服务器．修改后再输入续期的命令．续期成功后，把你的网站重新置于CDN的防护下．


[1]: http://lepig.cn/usr/uploads/2016/03/555677695.png

