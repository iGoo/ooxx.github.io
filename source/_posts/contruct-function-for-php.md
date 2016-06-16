---
title: PHP构造函数
s: contruct-function-for-php
date: 2016-06-16 18:03:21
tags: [PHP, 构造函数]
categories: PHP
---

## 构造函数
PHP5允许在一个类中定义一个函数作为类实例化时自动调用的函数，称之为`构造函数`
> PHP 5允行开发者在一个类中定义一个方法作为构造函数。具有构造函数的类会在每次创建新对象时先调用此方法，所以非常适合在使用对象之前做一些初始化工作。

> Note: 如果子类中定义了构造函数则不会隐式调用其父类的构造函数。要执行父类的构造函数，需要在子类的构造函数中调用 parent::__construct()。如果子类没有定义构造函数则会如同一个普通的类方法一样从父类继承（假如没有被定义为 private 的话）。

<!-- more -->

为了实现向后兼容性，如果 PHP 5 在类中找不到 __construct() 函数并且也没有从父类继承一个的话，它就会尝试寻找旧式的构造函数，也就是和类同名的函数。因此唯一会产生兼容性问题的情况是：类中已有一个名为 __construct() 的方法却被用于其它用途时。

与其它方法不同，当 __construct() 被与父类 __construct() 具有不同参数的方法覆盖时，PHP 不会产生一个 E_STRICT 错误信息。

自 PHP 5.3.3 起，在命名空间中，与类名同名的方法不再作为构造函数。这一改变不影响不在命名空间中的类

```php
namespace Foo;
class Bar {
    public function Bar() {
        // treated as constructor in PHP 5.3.0-5.3.2
        // treated as regular method as of PHP 5.3.3
    }
}
```


**注意： PHP不支持函数重载，所以构造函数不能存在多个**
Java可以重载，比如
```java
class Cat
{
    private string name = "";
    public Cat(string name)
    {
        this.name = name;
    }

    public Cat() //这里就已经重载了上面的同名方法
    {
        this.name = "无名";
    }

    public string Shout()
    {
        return "我的名字叫" + name + " (>^ω^<)";
    }
}
```

---

## 析构函数
> PHP 5 引入了析构函数的概念，这类似于其它面向对象的语言，如 C++。析构函数会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行。

```php
class MyDestructableClass {
   function __construct() {
       print "In constructor\n";
       $this->name = "MyDestructableClass";
   }

   function __destruct() {
       print "Destroying " . $this->name . "\n";
   }
}

$obj = new MyDestructableClass();
```

和构造函数一样，父类的析构函数不会被引擎暗中调用。要执行父类的析构函数，必须在子类的析构函数体中显式调用 parent::__destruct()。此外也和构造函数一样，子类如果自己没有定义析构函数则会继承父类的。

析构函数即使在使用 exit() 终止脚本运行时也会被调用。在析构函数中调用 exit() 将会中止其余关闭操作的运行。

> Note:
析构函数在脚本关闭时调用，此时所有的 HTTP 头信息已经发出。脚本关闭时的工作目录有可能和在 SAPI（如 apache）中时不同。
> Note:
试图在析构函数（在脚本终止时被调用）中抛出一个异常会导致致命错误。


**参考**
http://php.net/manual/zh/language.oop5.decon.php