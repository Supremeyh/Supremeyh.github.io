---
title: 面试题 JavaScript
comments: true
date: 2019-03-19 22:19:20
categories: Review
tags: ['Review', 'JS']
---

1. JavaScript 有哪些数据类型
6种原始数据类型：Boolean、Number、String、Null、Undefined、Symbol: 符号(Symbols)是ECMAScript第6版新定义的。符号类型是唯一的并且是不可修改的
引用类型：Object

2. 怎么判断不同的JS数据类型
* typeof操作符：返回一个字符串，表示未经计算的操作数的类型。
对于基本数据类型（除null外，被认为是一个空的对象引用），返回其本身的数据类型，函数对象返回 function ，其他对象均返回 Object。

* instanceof  用来判断A 是否是 B的实例，表达式为 A instanceof B，返回一个Boolean类型的值
测试构造函数的prototype属性是否出现在对象的原型链中的任何位置
instanceof 检测的是原型,只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型

* Object.prototype.toString  返回当前对象对应的字符串形式。默认情况下返回类型字符串
如，Object.prototype.toString.call(true) ; // [object Boolean]

