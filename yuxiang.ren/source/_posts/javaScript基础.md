---
title: javaScript基础
date: 2017-05-22 09:00:05
categories: HTMl
tags: javaScript html
---

# 一、函数

##  1. 函数的定义

1. 使用`function`定义

   ```javascript
   function fun() {
     alert("hello world");
   }
   ```

2. 匿名函数的方式

   ```javascript
   // 定义函数
   var fun = function() {
     alert("hello wordl");
   }
   // 执行
   fun();
   ```

3. `new Function`(不常用)

   ```javascript
   var fun = new Function("msg", "alert(msg)");
   fun("hello world");
   ```

4. 函数调用

   * 调用函数的时候是通过函数名调用的,
   * 定义函数的时候不能聪明.

<!-- more -->

## 2. 函数劫持

改变`javascript`的函数预定义好的功能

```javascript
window.alert = function(msg) {
  	document.write(msg)
}
alert("hello world");
```

## 3. 全局函数

1. `isNaN`: 是不是一个数字, 返回true不是数字
2. `parseInt`, `parsetFloat`
3. `eval`: 
   * 主要执行字符串,奖结果转换为数字
   * 将`json`格式的字符串转换为`json`

# 

