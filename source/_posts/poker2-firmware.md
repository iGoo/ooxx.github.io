---
title: 给poker2刷了下固件
s: poker2-firmware
date: 2016-11-19 20:02:21
tags: [poker2, firmware, 固件]
categories: 杂乱
---


用了`poker2`一年多了，现在`poker3`以及`poker升级版`(poker2升级版)都发布了。对我来说唯一有些不爽的应该是方向键的问题了。一直用`FN+ASDW`来坐方向键，用了很长一段时间后发现也挺顺手的，至少在编码的时候双手都不用离开中心点了。
后来看了一个帖子可以通过背板后面的DIP开关调整FN的位置，于是把`CAPSLOCK`键调整为FN，然后就可以左手一个手操作上下左右更加方便了，但是目前还没习惯。
后来看了`poker3`的方向键如果可以按`CAP+JKLI`或者是`CAP+HJKL`(VIM方式)岂不是更好，所以才有了刷固件的想法。
<!-- more -->
![POKER2原始键位](http://segmentfault.com/img/bVcCuW)
上面是POKER2原始键位图

> CAP变FN帖子 这个是之前用的，可以完美过渡到MAC
> https://segmentfault.com/a/1190000000585559

---
> 声明：刷这个固件以后在MAC上就有问题了，键各种不听使唤，所以这个目前只能在Win下使用

参考了这个帖子:https://bbs.wstx.com/thread-534633-1-1.html
![键位图](http://7xoumt.com1.z0.glb.clouddn.com/194326ygzaz97z2i87v81o.jpg)


* 这里说的`1100`以及`0011`、`1010`是按照键盘后面DIP开关`4321`的顺序来的，也就是说`1100`就是`43ON`、`21OFF`，我选的是`0111`方案。就是把`CAPSLOCK`键变成`FN`
* 波动开关最后断电操作，然后刷完使用FN+R清空编码表
* 编程操作举例，以`CAP+H`为左键头为例。按右`ALT+左CTRL`进入编程模式，然后按`H`，接着按右`ALT+A`，然后按`CAP`，接着按右`ALT+左CTRL`就可以了。剩下的3个方向键和这差不多。
