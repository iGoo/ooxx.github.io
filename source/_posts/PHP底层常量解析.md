---
title: PHP底层常量解析
date: 2016-05-22 16:25:58
tags: [zval, 常量]
categories: PHP
---



ZE中常量的结构体

```c
typedef struct _zend_constant {
  zval value; //常量定义的值
  int flags; //是否对大小写敏感
  char *name; //常量名  *在C中代表指针
  uint name_leg; //常量名长度
  int module_number; // 系统自定义还是用户自定义
}
```



