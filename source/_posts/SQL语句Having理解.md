---
title: SQL语句Having理解
date: 2016-04-22 19:07:25
tags: [Having, MySQL]
categories: 数据库
---
基本上在写sql语句的时候很少使用`having`，大部分都是使用`where`条件进行查询。但是`having`和`where`看起来很相似，又有些区别。
`having`子句在查询过程中慢于聚合语句`(sum,min,max,avg,count)`.而where子句在查询过程中则快于聚合语句`(sum,min,max,avg,count)`。


<!--more-->


where子句事例
//查询出id大于10的，进行聚合语句
`select sum(num) from tableName where id > 10`

having子句事例
//查询借阅20本以上书的读者ID和借阅量
`select ReaderId,count(ReaderID) AS cnt from BorrowInfo group by uid having cnt > 20;`

这里就不能用where来写了，比如`where cnt > 20` 如果这样写是错的，因为where只能用于数据库里的字段，而这里的cnt是我们自己定义的，所以这也是弥补了在进行`group by`分组时where的不足。

参考：http://my.oschina.net/shajin/blog/156486