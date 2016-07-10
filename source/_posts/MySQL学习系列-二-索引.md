---
title: MySQL学习系列(二)索引
date: 2016-06-10 15:51:35
tags: [索引, MySQL]
categories: MySQL
toc: true
---








### 插入数据
** 插入单行数据 **
```sql
INSERT INTO `table_name` SET
    字段1=值1,
    字段n=值n;
```

**插入多行数据**
```sql
INSERT INTO `table_name`
    [(字段1, 字段2, 字段n)]
    VALUES
    (值1, 值2, 值n), (值1n, 值2n, 值3n);
```
<!-- more -->
**插入查询结果**
```sql
INSERT INTO `table_name`
    (字段1, 字段n)
    SELECT 字段a, 字段b FROM `table_name2` [WHERE condition];

    mysql> insert into t7 select * from t8;
    Query OK, 1 row affected (0.00 sec)
    Records: 1  Duplicates: 0  Warnings: 0
```

### 更新数据
```sql
UPDATE `table_name` set
    字段1=值1,
    字段2=值2,
    字段n=值n
[WHERE condition];
```

### 删除数据
```sql
DELETE FROM `table_name`
[WHERE condition];
```
> PS: `DROP` 用于删除一些数据对象，比如库、表、索引等
