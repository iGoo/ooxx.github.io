---
title: SublimeText3运行PHP代码
s: sublimetext-build-for-php
date: 2016-07-12 18:07:23
tags: [PHP]
categories: SublimeText
---

* 在菜单里 `Tool->Build System->New Build System`
* 把默认的
```
{
    "shell_cmd": "make"
}
```
<!-- more -->
    修改为
```
{
    "cmd": ["php", "$file"],
    "file_regex": "php$", 
    "selector": "source.php" 
}
```
* 然后保存在默认位置，文件名字为"php.sublime-build"
* 使用`Ctrl+B`或者OSX使用`Command+B`执行  


![执行结果](http://i4.piimg.com/1949/e6959e8399f00214.jpg)

> 最后一步执行过程中`Windows`需要配置一下环境变量   
> ![配置环境变量](http://i1.piimg.com/1949/04ed2691dd49cb1f.jpg)
