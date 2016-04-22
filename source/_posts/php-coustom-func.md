---
title: PHP自用自定义函数整理
s: php-coustom-func
date: 2016-04-22 19:04:39
tags: [PHP, 自定义函数, 自定义]
categories: PHP
---
```php
/***********************
**功能:将多维数组合并为一位数组
**$array:需要合并的数组
**$clearRepeated:是否清除并后的数组中得重复值
***********************/
function array_multiToSingle($array,$clearRepeated=false){
    if(!isset($array)||!is_array($array)||empty($array)){
        return false;
    }
    if(!in_array($clearRepeated,array('true','false',''))){
        return false;
    }
    static $result_array=array();
    foreach($array as $value){
        if(is_array($value)){
            array_multiToSingle($value);
        }else{
            $result_array[]=$value;
        }
    }
    if($clearRepeated){
        $result_array=array_unique($result_array);
    }
    return $result_array;
}
```
<!-- more -->
