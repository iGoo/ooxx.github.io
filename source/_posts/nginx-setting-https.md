---
title: Nginx配置https服务
s: nginx-setting-https
date: 2016-12-10 20:17:48
tags: [https, nginx]
categories: Nginx
---



把`keer.me`域名续费了下，然后前几天发现七牛也推出了https证书免费一年服务。看到https最近如此火热，已经成为了未来互联网趋势，索性申请个配置上。之前申请过`Let's Encrypt`只是到期后来没续了。

<!--more-->

不过这次申请的是阿里云的https证书，七牛、腾讯等提供的申请步骤及流程都大同小异。申请过程网上很多教程，这篇只简单的记录下`nginx`配置https服务。



申请后会得到一个压缩包，解压后会得到2个证书文件，类似于`yourdomain.pem`和`yourdomain.key`

将这2个文件放到服务器上，比如`/srv/keer.me/cert/keerme.pem`和`/srv/keer.me/cert/keerme.key`然后去`nginx`的配置文件中配置，修改大致如下

```sh
server
    {
        listen 443 ssl http2;
        #listen [::]:443 ssl http2;
        server_name keer.me www.keer.me;
        index index.html index.htm index.php default.html default.htm default.php;
        root  /srv/keer.me;
        ssl on;
        ssl_certificate /srv/keer.me/cert/keerme.pem; #这里对应申请的公钥
        ssl_certificate_key /srv/keer.me/cert/keerme.key; #这里对应申请的私钥
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
    }
```

这个时候如果访问`keer.me`则还是使用默认的http，那么如何访问http强制跳转到https上呢?

一般，我们常见的写两个 Server，即：

```sh
server {
    listen 80;
    server_name domain.com;
    rewrite ^(.*) https://$server_name$1 permanent;
}
server {
    listen 443 ssl;
    server_name domain.com;
    ssl on;
    # other
}
```

```sh
server {
    listen 80;
    server_name domain.com;
    return 301 https://$server_name$request_uri;
}
server {
    listen 443 ssl;
    server_name domain.com;
    ssl on;
    # other
}
```

还有一种方法，非常规手段，即利用 497 状态码。

当此虚拟机只允许 HTTPS 来访问时，用 HTTP 访问会让 Nginx 报 497 错误，然后利用 error_page 将链接重定向至 HTTPS 上，即：

```sh
server {
    listen 443 ssl;
    listen 80;
    server_name domain.com;
    ssl on;
    # other
    error_page 497 https://$server_name$request_uri;
}
```

配置完成后可以在https://www.ssllabs.com/ssltest/测试下ssl等级。

> 参考 https://cipherli.st/