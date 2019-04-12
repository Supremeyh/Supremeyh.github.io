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

2. toString() 返回一个表示该对象的字符串
* 语法
object.toString()

* 描述
每个对象都有一个toString()方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，toString()方法被每个Object对象继承。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中type是对象的类型。

Object.prototype.toString.call(null)   => '[object Null]'
Object.prototype.toString.call(undefined)   => '[object Undefined]'
Object.prototype.toString.call(Math)   => '[object Math]'
Object.prototype.toString.call({})   => '[object Object]'
Object.prototype.toString.call([])   => '[object Array]'


3. 双等号 == 运算规则
![双等号==运算规则](/images/==运算规则.jpg)

undefined == null，结果是true。且它俩与所有其他值比较的结果都是false
String == Boolean，需要两个操作数同时转为Number
String/Boolean == Number，需要String/Boolean转为Number
Object == Primitive，需要Object转为Primitive(具体通过valueOf和toString方法)

参考: https://zhuanlan.zhihu.com/p/21650547
