---
title: 你问我答 单线程JavaScript的异步机制与经典for循环
comments: true
date: 2019-04-10 14:00:00
categories: Review
tags: ['Review', 'JS']
---


# 单线程JavaScript的异步机制与经典for循环

```JavaScript
for (var i = 1; i <= 5; i++) {
  console.log(i);

  setTimeout(function timer() {
    console.log(i);
  }, i * 1000);
}
```
答案： 先同时输出 12345；再输出5个6，每隔一秒输出一个
原理： JavaScript 是单线程的，所有的任务排队，按顺序一一执行。异步执行可以实现多任务并发。
解析：等执行栈内同步的 for 循环执行结束出栈后，线程才会从任务队列里拉取异步的定时器的回调函数。根据作用域的工作原理，尽管循环中的五个函数是在各个迭代中分别定义的，但是它们都被封闭在一个共享的全局作用域中，因此实际上只有一个 i。

异步机制：
宿主环境为JavaScript创建线程时，会创建堆(heap)和栈(stack)， 堆内存储JavaScript对象，栈内存储执行上下文
同步任务：执行栈内，执行上下文的同步任务按序执行执行完即退栈。
异步任务：异步任务执行时，该异步任务进入等待状态（不入栈），与此同时，通知线程“当触发该异步事件的时候（或该异步操作响应返回时），需向任务队列插入一个事件”，当实际上异步事件触发或异步操作响应返回时，线程向任务队列插入相应的回调事件。当执行栈清空后，线程从任务队列取出一个事件消息，其对应异步任务（函数）结束等待状态，进入执行栈，执行回调函数。如果该事件消息未绑定回调，则执行完任务后退栈，这个消息会被丢弃。当线程空闲（即执行栈清空）时继续拉取消息队列下一轮消息（next tick，事件循环流转一次称为一次tick）。

修复：
1、错误的闭包
```JavaScript
for (var i = 1; i <= 5; i++) {
  (function() {
    setTimeout( function() {
      console.log(i);
    }, i*1000)
  })()
}
```
依然全是 6。因为，新加上的 IIFE 作用域是”空的”，它并没有自己的变量。 执行栈清空后，线程从任务队列里读取回调函数，它们还是引用那个唯一的全局变量i

2、正确的闭包
```JavaScript
for (var i = 1; i <= 5; i++) {
  (function() {
    var j = i // 在闭包作用域中，通过添加自己的变量，每次迭代都捕获i的副本
    setTimeout( function() {
      console.log(j);
    }, j*1000)         //至于时间这里，是i 是j无所谓
  })()
}
```
3、更简洁的闭包：
```JavaScript
for (var i = 1; i <= 5; i++) {
  (function(j) {
    setTimeout( function() {
      console.log(j);
    }, j*1000)
  })(i) 
}
```
3、ES6块作用域
```JavaScript
for (var i = 1; i <= 5; i++) {
    let j = i
    setTimeout( function() {
      console.log(j);
    }, j*1000)

}
```
4、直接在for循环头部里，每次迭代都声明一次。随后的每个迭代都会使用上一个迭代结束时的值来初始化这个变量
```JavaScript
for (let i = 1; i <= 5; i++) {
    setTimeout( function() {
      console.log(i);
    }, i*1000)

}
```