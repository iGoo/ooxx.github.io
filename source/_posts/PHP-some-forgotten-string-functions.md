---
title: PHP一些被遗忘的字符串函数
s: PHP some forgotten string functions
date: 2016-04-20 23:18:30
tags: [php, 字符串函数]
categories: PHP
---

PHP一些被遗忘的字符串函数
```php
ctype_alnum -- Check for alphanumeric character(s)
检测是否是只包含[A-Za-z0-9]
函数原型: boll ctype_alnum(string text);
```


```php
ctype_alpha -- Check for alphabetic character(s)
检测是否是只包含[A-Za-z]
函数原型: boll ctype_alpha(string text);
```

<!-- more -->

```php
ctype_digit -- Check for numeric character(s)
检查时候是只包含数字字符的字符串（0-9）
函数原型: boll ctype_digit (string text);
```
```php
ctype_cntrl -- Check for control character(s)
检查是否是只包含类是“ ”之类的字符控制字符
函数原型: boll ctype_cntrl (string text);
`未知原理`
```
```php
ctype_graph -- Check for any printable character(s) except space
检查是否是只包含有可以打印出来的字符（除了空格）的字符串
函数原型: boll ctype_graph (string text);
```

```php
ctype_lower -- Check for lowercase character(s)
检查是否所有的字符都是英文字母，并且都是小写的
函数原型: boll ctype_lower (string text);
```
```php
ctype_upper -- Check for uppercase character(s)
检查是否所有的字符都是英文字母，并且都是大写的
函数原型: boll ctype_upper (string text);
```
```php
ctype_print -- Check for printable character(s)
检查是否是只包含有可以打印出来的字符的字符串
函数原型: boll ctype_print (string text);
```
```php
ctype_punct -- Check for any printable character which is not whitespace or an alphanumeric character
检查是否是只包含非数字/字符/空格的可打印出来的字符
函数原型: boll ctype_punct (string text);
```
```php
ctype_space -- Check for whitespace character(s)
检查是否是只包含类是“ ”之类的字符和空格
函数原型: boll ctype_space (string text);
```
```php
ctype_xdigit -- Check for character(s) representing a hexadecimal digit
检查是否是16进制的字符串，只能包括“0123456789abcdef”
函数原型: boll ctype_xdigit (string text);
```
```php
if(!preg_match("/^[a-z\d]{6,15}$/i",$variable)){
    echo '密码必须为6-15位的数字和字母的组合';
}
```
> 这些小函数可以避免使用preg_match正则表达式

