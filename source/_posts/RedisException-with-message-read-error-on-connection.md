---
title: "解决'RedisException' with message 'read error on connection'"
s: RedisException-with-message-read-error on connection
date: 2016-06-22 17:41:38
tags: [phpredis, redis, RedisException]
categories: Redis
---

> 在使用`phpredis`扩展时，测试消息队列时用到了`brpop`和`blpop`这样的阻塞式命令。默认超过60秒以后会出现PHP异常

```sh
[root@localhost vagrant]# php queue.php 
PHP Fatal error:  Uncaught exception 'RedisException' with message 'read error on connection' in /vagrant/queue.php:43
Stack trace:
#0 /vagrant/queue.php(43): Redis->brPop('emailQueue', '0')
#1 /vagrant/queue.php(54): EmailQueue->emailWorker()
#2 {main}
  thrown in /vagrant/queue.php on line 43
```
<!-- more -->
以为是`redis`配置文件中设置了，查看了`redis.conf`后发现`timeout`默认已经设置为0

后来查看了一些资料发现由于`phpredis`底层默认使用的是php的`socket`方式,后来查看了`php.ini`发现`default_socket_timeout = 60`这一设置才明白。

不过不推荐直接修改`php.ini`文件，而是使用`ini_set()`函数来动态修改
```php
ini_set('default_socket_timeout', -1); // 不超时
```
---
后来发现有人向`phpredis`作者提交了一个Pull requests 见[https://github.com/phpredis/phpredis/pull/260](https://github.com/phpredis/phpredis/pull/260). 里面给出了一个定义`OPT_READ_TIMEOUT`的选项 这里我设置成`-1`同样可以
```php
$this->redis = new Redis();
$this->redis->pconnect('127.0.0.1', 6379);
$this->redis->setOption(Redis::OPT_READ_TIMEOUT, -1); //Note: You cannot setOption(Redis::OPT_READ_TIMEOUT, 0) to NOT time out. php_stream_set_option doesn't accept 0 for that purpose.
$this->redis->auth('xxxxxx');
$this->redis->select(15);
```


**参考资料**
https://github.com/phpredis/phpredis/issues/70
https://github.com/phpredis/phpredis/pull/260


