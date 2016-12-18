---
title: 比cURL更酷的http命令行客户端-HTTPieEEE
s: http-requests-for-httpie
date: 2016-12-15 21:38:10
tags: [httpie, curl, http]
categories: Tools
---

* 发送GET请求,打印`request headers`和`request body`
  ```bash
  http --print=BH my.sso.dev/request keywords==lePig
  ```

* 发送POST请求，打印`request headers`和`request body`
  ```bash
  http --print=BH my.sso.dev/request keywords=lePig
  ```

<!--more-->

* 只返回Response Header或Response Body
```
http -h my.sso.dev/request | http --print=h my.sso.dev/request
http -b my.sso.dev/request | http --print=b my.sso.dev/request
```

* 发送POST请求，并传递一个非字符串参数
```
http --print=BH my.sso.dev age:=27  colors:='["red", "green", "blue"]' name=lepig
{
    "age": 27,
    "colors": [
        "red",
        "green",
        "blue"
    ],
    "name": "lepig"
}
```
`:=`表示一个非string。即可以是bool值，也可以是数字，也可以是一个数组。查看`http --help`下的REQUEST_ITME项













![HTTPie](https://httpie.org/static/img/httpie2.png)




