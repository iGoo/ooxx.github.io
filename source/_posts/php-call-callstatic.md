---
title: PHP魔术方法__call()和__callStatci()深入理解
s: php-_call-_callstatic
date: 2016-11-13 23:00:18
tags: [__call, __callStatic]
categories: PHP
---
### PHP可以使用静态方式调用非静态方法引入的思考
在看某个ORM类库的时候发现子类使用了`subClass::parentMethod()`的方式调用,比如`self::save()`，在记忆里这是静态调用，可是这个`save()`方法在父类里是一个非静态方法。于是刨根问底的搜索问题答案才有了下面的内容。
<!-- more -->
### 魔术方法定义
> __call(string $name, array $arguments)
在**对象**中调用一个**不可访问**方法时,__call()会被触发.

> __callStatic(string $name , array $arguments)
使用**静态方式**调用一个**不可访问**的方法时,__callStatic()会被触发. 如果调用的方法是非静态方法，那么会报一个`Strict Standards`提示。


```
class A
{
    public function __call($name, $args)
    {
        echo "NO\n";
    }

    public static function __callStatic($name, $args)
    {
        echo "YES\n";
    }
}
class B extends A
{
    public function test()
    {
        A::test();
    }
    
    public static function stest()
    {
        A::test();
    }
}

$b = new B; $b->test(); //执行调用
```
1. 先看B继承A的情况
    * 当执行到test方法中的`A::test()`的时候，会去A类中找这个方法，如果找不到恰好又声明了`__call()`和`__callStatic()`的话`__call()`就会被调用。那么问题来了，我们明明是用`A::test()`进行静态调用的，为什么会`__call()`会被调用，应该是`__callStatic()`被调用啊混蛋!!!
    * 这个时候看了[PHP中的calling scope](http://www.laruence.com/2012/06/14/2628.html)这篇文章以后，对calling scope有了比较深入的理解。首先明确了在PHP中, 判断静态与否不是靠`::`(PAAMAYIM_NEKUDOTAYIM)符号, 而是靠calling scope，而`$this`指针在方法被调用的时候指向的就是这个calling scope。所以当执行`A::test()`的时候，B对象的calling scope就被传递给了A，那么这个时候就不是静态调用，所以执行了`_call()`方法。
2.  再看B没有继承A的情况
    * 当执行到test方法中的`A::test()`的时候，相当于类外调用。那么这个时候可以分两种情况讨论
    * 如果A类里没有`test()`方法，就如同上面示例代码，那么这时会在A类里寻找`test()`，没有则进入到`__callStatic()`中。如果安装1的情况那么理解应该是进入`__call()`。所以我的理解是如果A中没有存在所对应的方法，那么此时这个作用域就丢失了，所以就进入了`__callStatic()`中。   
    * 如果A类中有`test()`方法，那么这个就和鸟哥那篇博客里举的例子一样了，这时候也会把B类的作用域传递到被调用的`test()`方法中。但是这里PHP5(Strict Standards错误)和PHP7(Deprecated错误)有些不一样，我将在下一篇记录一下。
---
### 参考阅读

> 1. http://www.laruence.com/2012/06/14/2628.html
> 2. http://www.cnblogs.com/yjf512/archive/2012/09/12/2682556.html
> 2. http://www.cnblogs.com/hechunhua/p/3909574.html
> 3. https://segmentfault.com/q/1010000000095833
> 4. http://www.chanxiaoxi.me/2015/11/18/static-call-non-static-method-in-php/

