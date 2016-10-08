---
title: 字符串加载更多显示方式
s: display-string-way
date: 2016-09-13 22:10:13
tags: [省略号，css]
categories: PHP
---

“阅读更多”这个功能相信大家都遇到过，即摘要内容如果过多的情况下让其显示`...`
<!-- more -->
但是什么方式最好最优雅呢？
1. 通过后端截取字符串的方式
    以PHP为例。使用`substr`或者是`mb_substr`函数来进行截取返回给前端显示。
    ```php
    echo substr('我是一个将要bei截取的字符串..aaaAAAbbb,,,.。', 0, 10);
    ```
    OR
    ```php
    echo mb_substr('我是一个将要bei截取的字符串..aaaAAAbbb,,,.。', 0, 10);
    ```
    上面两种方式截取出来的字符串如果内容中包含中英文等字符会导致截取的字符长度不一样，也就导致了在前端显示的时候长短不一，很不美观。所以还是推荐使用下面一种方式，也很灵活。
2. 前端通过CSS样式来控制

```css
overflow:hidden; // 溢出部分隐藏
text-overflow:ellipsis; // 文本不进行换行
white-space:nowrap; //当文本溢出包含元素时显示省略号
```
