---
title: PHP UUID扩展安装
date: 2016-06-14 18:41:48
tags: [PHP, uuid, 扩展]
categories: PHP
---

> 在使用到订单业务的时候生成唯一订单号的问题有些纠结，参看了网上一些生成订单流水号的方法感觉还行。
> 
> 但是在Google的同时发现PHP生成uuid的时候可以直接使用扩展来生成非常的方便。
> 记录下安装流程及简单的使用方法

<!-- more -->

## 下载安装扩展
```sh
wget http://pecl.php.net/get/uuid-1.0.4.tgz
tar zxvf uuid-1.0.4.tgz
cd uuid-1.0.4
phpize
./configure --with-php-config=/usr/local/php/bin/php-config #指定PHP安装路径
make && make install
```
**查看扩展是否安装成功**
```sh
php -m | grep uuid
```

## 注意事项
1. 我自己在CentOS7.2上编译的时候丢了2个依赖库，分别是`re2c`和`libuuid-devel`
2. 安装依赖库 `yum install re2c -y`和`yum install libuuid-devel`

## 代码调用
```php
function create()
{
    uuid_create(); # b6489492-4706-4d68-a52b-63e2bbddfbf3
    uuid_create(1); # 908c13b0-321e-11e6-97d9-080027c23ce5
}
```
---
PS:多刷新几次以后发现参数为`1`的有些段是一样的，具体UUID生成规则还得细细看一下。



