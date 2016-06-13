---
title: SublimeText插件备忘
s: SublimeText Plugins1111
date: 2016-06-12 23:19:45
tags: [Tools, SublimeText]
categories: Tools
---
<!-- <img src="http://static.keer.me/o_1al2jldppaod1eja1cgk1aif1a3ma.png?imageView2/1/w/700/h/400" alt="o_1al2jldppaod1eja1cgk1aif1a3ma.png" > -->
![o_1al4jht341rl53sjvf6ql5ne3a.jpg](http://static.keer.me/o_1al4jht341rl53sjvf6ql5ne3a.jpg)
> 整理平时常用的一些`SublimeText`常用的开发插件，不定期更新

<!-- more -->

### ConvertToUTF8
中文编码GBK转换为UTF8，直接CMD+Shitf+P搜索安装即可


### SideBarEnhancements
增强ST侧边栏功能`必备`插件

### Alignment
一款代码格式对齐插件，默认快捷键是ctrl+cmd+a如果使用QQ建议改一下QQ的快捷键。因为这3个键如果在Windows平台上我使用的是ctrl+alt+a和OSX上面位置一样，用着很顺手。
```php
    var name = "sublimt"
    var version = "2.0.1"
    var title = "sublime text"
    -----格式化后-----
    var name    = "sublimt"
    var version = "2.0.1"
    var title   = "sublime text"
```

### Function Name Display
看名字就是知道，这是一款可以在状态栏左下角显示当前方法函数名或者是方法名的一个小插件，直接安装即可

### AdvancedNewFile [链接](https://github.com/skuroda/Sublime-AdvancedNewFile)
在新建文件或者是文件夹的时候一般都是都是右键侧边栏进行操作。但是安装了这个插件以后就可以类似的模拟linux下的建立文件方式了。
只要使用OSX下使用`cmd+alt+n`即可打开输入框 win下使用`ctrl+alt+n`
> 如果一个侧边栏中有多个project那么直接数据project name就可以定位到此project，注意大小写
> ![AdvancedNewFile](http://static.keer.me/o_1al4j2esg1ksu18i3gksjr5siha.png)

### ctags [链接]()
代码跳转神器。因为不太喜欢IDE的臃肿所以这个插件对于ST来说也是`必备`插件
1. 使用cmd+shift+p安装ctags插件
2. 安装ctags程序
  1. http://ctags.sourceforge.net 下载安装包
  2. 下载ctags-5.8.tar.gz后解压编译。编译3步走，不在介绍
3. 简单配置一下sublimetext ctags。默认ctags安装在了`/usr/local/bin/ctags` 打开
    ![ctags setting](https://i.niupic.com/images/2016/06/05/ovKaUS.png)
    将"command"设置为`/usr/local/bin/ctags` `"command": "/usr/local/bin/ctags",`
4. 参考 [Mac OSX下Sublime Text配置使用Ctags实现代码跳转](http://www.smslit.top/develop/2015/11/14/macSTctags-Develop.html)

### Codeigniter Utilities
> CI框架辅助插件，可以直接输入`ci`然后进行选择进行快速补全

### DocBlockr
> 函数方法注释
可以进行配置，以便于定制要生成的注释、比如可以自定义方法创建的日期和作者名称

### Bracket​Highlighter [链接](https://github.com/facelessuser/BracketHighlighter)

具体设置可以参看官方文档


### All Autocomplete [链接](https://github.com/alienhard/SublimeAllAutocomplete)
> 比自带的快速补全更强大，他可以在所有打开的tab中补全你的当前输入

###  sublimelinter

`"show_errors_on_save": true,` 这个选项修改为`true`在保存的时候提示错误行号
`paths`里填写php路径
```json
"paths": {
            "linux": [],
            "osx": ["/usr/bin"],
            "windows": []
        },
```

参考：https://www.onlyke.com/html/404.html













  [1]: /img/bVxQ88