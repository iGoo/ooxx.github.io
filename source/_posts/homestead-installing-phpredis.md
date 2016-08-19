---
title: Homestead PHP7安装phpredis扩展
s: homestead-installing-phpredis
date: 2016-08-09 11:25:07
tags: [Homestead, phpredis, extension]
categories: PHP
---

之前工作已经家里PHP开发环境一直也是使用的`Vagrant`，不过LNMP的环境是自己配置的。最近离职以后在家折腾了一下`Homestead`PHP开发环境，发现对于新手来首真的是开箱即用。一些在自己配置环境遇到的问题，比如`index.php`地址重写的问题`Homestead`已经在nginx配置里为我们配置好了。所以如果你依然在使用WAMP或者说是MAMP等开发集成环境，强烈建议切换到linux环境中来。

我使用的是目前的最新版本的`Laravel Homestead version 3.0.2`。虽然她集成了众多的开发环境，比如`Memcache`,`Nodejs`,`Git`等，但是对于PHP开发者来说`Redis`这好东西绝对是不能缺少的，所以才有了这篇简短的纪要。

## 安装前装备
下载最新的phpredis扩展包[下载链接](https://github.com/phpredis/phpredis/releases) 这里注意一下，由于我使用的是最新的PHP7，所以phpredis扩展应该下载3.x.x，因为2.x.x系列的对php支持不友好。如果你使用的是php5请下载2.x.x的最新版本。
* 解压下载的源码包
    ```sh
    sudo tar zxvf 3.0.0.tar.gz
    ```

* 进入源码包文件，使用phpize初始化编译
    ```
    cd phpredis-3.0.0/
    phpize
    ```

## 编译安装
* 完成以上操作后会在当前文件夹中生成一个`configure`的可执行文件，这个文件应该都很熟悉。

* 编译三步走
    ```
    sudo ./configure
    make
    sudo make install
    ```

## 模块导入到PHP配置
在我之前自己配置的LNMP环境中，到这一步的时候应该是在`php.ini`文件中追加一行`extension=redis.so`, 不过看了`Homestead`的PHP配置文件采用的是软连接的方式。不过在`php.ini`文件中追加也是可以的。
但是我还是希望按照`Homestead`的方式来引入`redis.so`

* 创建redis.ini
    ```
    sudo vim /etc/php/7.0/mods-available/redis.ini
    # 然后在这个文件中写入`extension=redis.so` 保存，退出
    ```

* 引入redis.ini
    ```
    cd /etc/php/7.0/fpm/conf.d
    sudo ln -s /etc/php/7.0/mods-available/redis.ini 25-redis.ini
    ```
    在这个conf.d中建立的所有`*.ini`文件都在PHP初始化的时候自动加载。不过目前不太明白的是`25-redis.ini`这里的25是具体该定义成什么。不过我看了自带了一个`25-memcache.ini`所以我把redis也定义成25。(优先级序号)

* 然后重启php-fpm
    ```
    sudo /etc/init.d/php7.0-fpm reload
    ```
    到此为止通过浏览器即fpm的配置已经可以了，但是如果是cli还是得稍微配置一下步骤和上面的基本一样，也是在cli文件下的conf.d中穿件一个软链接，如果图省事就直接在cli文件夹下的php.ini最后一行直接追加吧。

---
网上搜索了一下别人的做法是直接在conf.d文件夹下直接创建了`redis.ini`而不是使用软连接的方式。其实都一样，不过为了统一我还是使用了软连接的方式的。
参考
> http://codegists.com/code/vagrant%20homestead%20php7%20redis/
> https://laracasts.com/discuss/channels/tips/tutorial-guide-installing-php-redis-on-fresh-install-homestead-with-php7





