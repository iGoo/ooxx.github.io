---
title: PHP随机数生成畅想
s: php-random-generator
date: 2016-08-19 22:33:14
tags: [uuid, guid, random, 随机数]
categories: PHP
---

## 说明
在开发中常常会用到生成一些`随机密码`、`推荐码`以及一些随机数等等。那么我在开发的过程中用过很多种方法。有基于**时间戳**、**随机函数**以及**UUID**等等一些方法。这篇是对于目前已经用过或者学习到的一些小例子的整理，方便以后用到的时候可以快速的查阅。
<!-- more -->

## 示例
* **fread**
    读取linux系统中的随机设备来生成随机码
    ```php
        echo bin2hex(fread(fopen('/dev/urandom', 'r'), 16));
        //output: 1e363b0cf43e8059b13c8f55db2b8653
    ```
    在[paragonie/random_compat](https://github.com/paragonie/random_compat/blob/master/ERRATA.md)这个随机库中，fread的优先级要高于下面2个函数的优先级。

* **mcrypt_create_iv**
    此函数依赖于Mcrypt扩展。在PHP5.6.0以前如果不指定第二个参数，那么默认使用linux系统中的`/dev/random`作为随机数产生器，之后的已经默认使用了`/dev/urandom`
    ```
        echo bin2hex(mcrypt_create_iv(16));
        //output: 3f2d9e1405137674ab5b710860f0377f
    ```
    > http://www.laruence.com/2012/09/24/2810.html
    > http://php.net/manual/zh/function.mcrypt-create-iv.php
    > http://blog.wpjam.com/m/wpjam-mcrypt 待研究

---
* **openssl_random_pseudo_bytes**
    此函数返回指定长度的字节流，然后可以使用`bin2hex`转换为16进制的字符串.此函数需要安装对应的php-openssl扩展，不过一般的php环境都会安装这个扩展
    ```php
    echo bin2hex(openssl_random_pseudo_bytes(16));
    //output: 969ed13182e00505dbbcb8550659b4d7
    ```

---
* **file_get_contents**
    对于Linux系统来说这个可以直接读取Linux系统运行时的运行文件来获取一个UUID
    ```
    echo file_get_contents('/proc/sys/kernel/random/uuid');
    // 输出: 6c509fc7-328b-4d82-9af8-60c44e7b84b4
    ```

---
* **字符池随机**
    也就是把一些字符串加入到一个池中，然后循环的方式从里面随机挑选进行组合生成。
    ```php
        function randomkeys($length)
        {
            $pattern = '1234567890abcdefghijklmnopqrstuvwxyz
                        ABCDEFGHIJKLOMNOPQRSTUVWXYZ,./&l
                        t;>?;#:@~[]{}-_=+)(*&^%$£"!';    //字符池
            for($i=0;$i<$length;$i++)
            {
                $key .= $pattern{mt_rand(0,35)};    //生成php随机数
            }
            return $key;
        }
        echo randomkeys(8);
    ```
    另一种方法稍微做了些修改，利用chr()函数已经[ASCII表](http://www.asciitable.com)，省去创建字符池的步骤。
    ```php
        function randomkeys($length)
        {
            $output='';
            for ($a = 0; $a < $length; $a++) {
                $output .= chr(mt_rand(33, 126));    //可以参看ASCII表设置字符的范围
            }
            return $output;
        }
        echo randomkeys(8);
    ```
    上面2个函数功能一样，只是第二个更加的简洁 

---
* **邀请码**
    在之前的开发中生成一个邀请码的需求，要求数字加字母，参考了一些别人的推荐码都是`数字` `大小写字母` 长度一般在8-10位左右,比如**R5a7Qed3**
    ```php
        passwordChar = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
        $passwordLength = 8;
        $max = strlen($passwordChar) - 1;
        $password = '';
        for ($i = 0; $i < $passwordLength; ++$i) {
            $password .= $passwordChar[mt_rand(0, $max)];
        }
        echo $password;
        //possible output: 7dgG8EHu
    ```









## 文章参考
> https://segmentfault.com/a/1190000004182573

## 类库参考
> https://github.com/paragonie/random_compat
> 这个库提供了只有在PHP7上才有的随机函数`random_int`和`random_bytes`，如果你想在PHP5中使用这2个函数那么可以使用这个库

---
> https://gist.github.com/dahnielson/508447
>  一个github上别人写的UUID v3 v4 v5生成类

---
> http://stackoverflow.com/questions/2040240/php-function-to-generate-v4-uuid
> http://qa.helplib.com/296097 翻译的版本
> 这里面也是一些生成UUID的方法，有些可以借鉴一下

---
> https://github.com/ramsey/uuid
> 依然是个生成UUID的库

---
> http://hashids.org/
> 新发现的，待研究
