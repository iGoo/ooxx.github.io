---
title: PHP生成订单号方法汇总
date: 2016-06-15 17:23:33
tags: [PHP, 订单号]
---

> 项目使用到了订单号生成的需求，看了一遍基本上是时间戳加随机数的思路。
> 于是找了一些`PHP`生成订单流水号的具体代码，作为记录。

### (一) 从`ECSHOP`中扒了一段订单号生成代码
```php
function build_order_no()
{
    /* 选择一个随机的方案 */
    mt_srand((double) microtime() * 1000000);
    /* 日期前缀+一个1-999999的六位随机数不满6未前位补0 */
    return date('ymd') . str_pad(mt_rand(1, 999999), 6, '0', STR_PAD_LEFT);
}
```
本机(cpu:i5 mem:1G)测试生成10万订单号、重复代码在1万左右，对于目前系统业务来说绝对够用了。
> 如果不放心可以在生成订单号后查询下数据库是否有对应的order_no如果有重新生成

<!-- more -->

### (二) 无碰撞方法
```php
function build_order_no() {
    $year_code = array('A','B','C','D','E','F','G','H','I','J');
    $order_sn = $year_code[intval(date('Y'))-2010].
    strtoupper(dechex(date('m'))).date('d').
    substr(time(),-5).substr(microtime(),2,5).sprintf('%02d',rand(0,99));
    return $order_sn;
}
```
说无碰撞有点夸张、但是碰撞概率极低，依然是本机测试10万号全部没有碰撞。如果不想使用字符开头的符号可以注释掉1、2行只用后面的部分  
从生成的结果来看这段代码生成的订单号是自增的，所以一般情况下不会出现重复，虽然是自增，但是不会像MySQL主键自增那样有规律的自增，所以一般中小型网站还是可以使用的。

### (三) 一个碰撞率有点高的生成方法
```php
function build_order_no(){
    return date('Ymd').substr(implode(NULL, array_map('ord', str_split(substr(uniqid(), 7, 13), 1))), 0, 8);
}
```
本机(同上)测试生成10万订单重复的大概有65000左右，所以基本上不推荐使用

---
### 附上一个GUID生成方法
```php
public function guid() {
    if (function_exists('com_create_guid')) {
        return com_create_guid();
    } else {
        mt_srand((double)microtime()*10000);
        $charid = strtoupper(md5(uniqid(rand(), true)));
        $hyphen = chr(45);
        $uuid   = chr(123)
            .substr($charid, 0, 8).$hyphen
            .substr($charid, 8, 4).$hyphen
            .substr($charid,12, 4).$hyphen
            .substr($charid,16, 4).$hyphen
            .substr($charid,20,12)
            .chr(125);
        return $uuid;
    }
}
```

