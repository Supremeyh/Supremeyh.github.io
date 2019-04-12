---
title: 'JavaScript 疑难杂症,专治各种不服'
comments: true
date: 2019-04-12 15:25:45
categories: JS
tags: ['JS']
---

1. valueOf() 返回指定对象的原始值
* 语法
object.valueOf()

* 不同类型对象的valueOf()方法的返回值
Array	返回数组对象本身。
Boolean	布尔值。
Date	存储的时间是从 1970 年 1 月 1 日午夜开始计的毫秒数 UTC。
Function	函数本身。
Number	数字值。
Object	对象本身。这是默认情况。
String	字符串值。
Math 和 Error 对象 没有 valueOf 方法。

* 可覆盖Object.prototype.valueOf()来调用自定义方法

