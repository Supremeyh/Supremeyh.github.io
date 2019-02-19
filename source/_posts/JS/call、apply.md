---
title: call、apply
comments: true
date: 2019-02-08 22:00:56
categories: JS
tags: ['JS']
---

一般来说，this总是指向调用某个方法的对象，但是使用call()和apply()方法时，可以用来重新定义函数的执行环境，就会改变函数体内this的指向。
即call()和apply() 就是用来让括号里的对象来集成括号外的函数的属性！可以称之为继承！ 


* func.call(thisObj, arg1, arg2, ...)
thisObj，在fun函数运行时指定的this值。需要注意的是，指定的this值并不一定是该函数执行时真正的this值，如果这个函数处于non-strict mode，则指定为null和undefined的this值会自动指向全局对象(浏览器中就是window对象)，同时值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象。

返回值：使用调用者提供的this值和参数调用该函数的返回值。若该方法没有返回值，则返回undefined。


* func.apply(thisObj, [argsArray])
接收两个参数，一个是函数运行的作用域（this），另一个是参数数组。
thisObj，可选的。在 func 函数运行时使用的 this 值。请注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。

argsArray,可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为 null 或  undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。


```
function add(a,b) {  
    alert(a+b);  
}  

function sub(a,b) {  
    alert(a-b);  
}  

add.call(sub,1,1);  //  2
 ```


* Array.apply() 创建数组
Array.apply(null, {length: 4}): [undefined, undefined, undefined, undefined] // 初始化全为undefined
Array.apply(null, {0:'a', 1: 'b', 2: 'c', 3: 'd', length:5}))  // 赋值 ["a", "b", "c", "d", "e"]
Array(4): [empty × 4] // 只占位


 * Array.prototype.slice.call() 能将具有length属性的对象转成数组
 将函数的实际参数转换成数组的方法：
 let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
}

 let args1 = Array.prototype.slice.call(arguments)
 let args2 = [].slice.call(arrayLike)
 let args3 = Array.from(arrayLike)

function example( arg1, arg2, arg3 ) { 
  return Array.prototype.slice.call(arguments, 1);  // Returns [arg2, arg3] 
}

