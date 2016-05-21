---
title: PHP使用Session保存临时注册用户数据
date: 2016-05-18 23:07:18
tags: [PHP, Session, Cookie]
---

### 引言

> 用户登录注册模块改版，产品原型说明的是普通用户注册分为两个页面流程，导致到第二个页面的时候需要把第一步填写的一些信息传入到第二个页面中。
>
> 刚开始准备放到Redis中，能很好的解决问题。但是后来还是放到了Session中，因为Session使用的也是Redis存储。二来在Codeigniter 3中也很好的支持Session操作。



<!-- more -->



![注册cookie.jpg](https://ooo.0o0.ooo/2016/05/19/573daa6419ad2.jpg)