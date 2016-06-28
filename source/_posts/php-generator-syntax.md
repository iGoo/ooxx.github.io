---
title: PHP轻量的集合迭代器
s: php-generator-syntax
date: 2016-06-23 13:21:06
tags: [yield, 生成器, 迭代器]
categories: PHP
---



PHP5中的生成器不允许有返回值，否则会报错
```php
PHP Fatal error:  Generators cannot return values using "return" in /vagrant/queue.php on line 64
```
然而在最新版本的PHP7中已经增加了返回值这一特性，可以使用`getRetrun()`进行获取.
```php
public function makeRangeByGenerator($range)
{
    for ($i = 0; $i < $range; $i++) {
        yield $i;
    }

    return 'Generate Finishied'; //php7可以定义返回值
}

$result = makeRangeByGenerator(1000);
// 输出返回值
$result->getReturn(); // Generate Finishied
```




参考资料
> https://segmentfault.com/a/1190000004307491