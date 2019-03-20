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

3. 怎么实现对对象的拷贝(浅拷贝与深拷贝)
* 浅拷贝。 拷贝原对象的引用，复制引用（指针），而未复制真正的值，改变原对象以后，新对象跟着改变。
var obj1 = obj2   赋值运算符= 
Array.prototype.slice() 
Array.prototype.concat() 
Object.assign()  只拷贝对象自身的可枚举的属性

* 深拷贝
JSON.parse(JSON.stringify(obj)。 复合类型的值只能是数组或对象，不能是函数、正则、日期对象，也不能是undefined
展开运算符...   取出参数对象的所有可遍历属性，拷贝到当前对象之中
immutable  改变新的对象不会改变原对象，在大量深拷贝操作中显著地减少性能消耗
递归来实现每一层都重新创建对象并赋值，真正意义上的深拷贝
```JavaScript
function deepClone(source){
  const targetObj = source.constructor === Array ? [] : {}; // 判断复制的目标是数组还是对象
  for(let keys in source){ // 遍历目标
    if(source.hasOwnProperty(keys)){
      if(source[keys] && typeof source[keys] === 'object'){ // 如果值是对象，就递归一下
        targetObj[keys] = source[keys].constructor === Array ? [] : {};
        targetObj[keys] = deepClone(source[keys]);
      }else{ // 如果不是，就直接赋值
        targetObj[keys] = source[keys];
      }
    }
  }
  return targetObj;
}
```
