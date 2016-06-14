---
title: 支付宝即时到帐接入
s: alipay-direct-pay
date: 2016-06-14 17:47:23
tags: [PHP, alipay, direct]
categories: PHP
---

> 网站简单账户系统开发，需要充值功能，优先接入支付宝的即时到帐功能，很简单。做下记录

<!-- more -->
![账户余额](http://static.keer.me/o_1al7578161m96k3f11ki13o1qgba.png)
## 介绍
整个围绕账户系统开发的一些功能如上图，除了充值是和支付宝有交互以外，其他都使用网站虚拟货币积分进行交易操作。   
所以整个关键点就在`即时到帐`那一环上面。

## 准备工作
1. 已经开通即时到帐功能的支付宝帐号 [链接](https://doc.open.alipay.com/doc2/detail.htm?spm=a219a.7629140.0.0.K22k1k&treeId=62&articleId=104749&docType=1)
2. 下载PHP-SDK包 [链接](http://aopsdkdownload.cn-hangzhou.alipay-pub.aliyun-inc.com/demo/alipaydirect.zip?spm=a219a.7629140.0.0.lDGfHx&file=alipaydirect.zip)
```bash
» tree .
├── alipay.config.php
├── alipayapi.php
├── cacert.pem
├── img
│   ├── alipay_logo.png
│   ├── guanzhu_qrcode.png
│   └── little_qrcode.jpg
├── index.php
├── lib
│   ├── alipay_core.function.php
│   ├── alipay_md5.function.php
│   ├── alipay_notify.class.php
│   └── alipay_submit.class.php
├── log.txt
├── notify_url.php
├── readme.txt
└── return_url.php
```
 其中如果使用MD5的话只需要 `alipay_core.function.php`、`alipay_notify.class.php`、`alipay_submit.class.php`

3. 配置PID和APPKEY
 我使用的是`CI`框架所以我的配置全部放在了`APPPATH . config/alipay.php`中，类似下面这样
```php
   $config['partner']   = '2088xxxxx';
   // 安全检验码，以数字和字母组成的32位字符
   $config['key']       = '8uva4yji8bpdfat1wkgxxxxxxxxxxxx';
   .
   .
   .
``` 


4. AAAAAAAAAAAAAAAAAA


## 代码实现