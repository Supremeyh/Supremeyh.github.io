---
title: JavaScript基础
comments: true
date: 2019-02-26 09:52:37
categories: JS
tags: ['JS', '基础']
---

### 概述
* 什么是 JavaScript 语言？
JavaScript (JS for short) is the programming language that enables web pages to respond to user interaction beyond the basic level. It was created in 1995 by Brendan Eich, and is today one of the most famous and used programming languages.

JavaScript 是一种轻量级的脚本语言。所谓“脚本语言”（script language），指的是它不具备开发操作系统的能力，而是只用来编写控制其他大型应用程序（比如浏览器）的“脚本”。

JavaScript 也是一种嵌入式（embedded）语言。它本身提供的核心语法不算很多，只能用来做一些数学和逻辑运算。JavaScript 本身不提供任何与 I/O（输入/输出）相关的 API，都要靠宿主环境（host）提供，所以 JavaScript 只合适嵌入更大型的应用程序环境，去调用宿主环境提供的底层 API。

> 编译型语言 vs 解释型语言
编译型语言，首先是将源代码编译compile生成机器指令，再由机器运行机器码 (二进制)。执行效率高。如C、C++
解释型语言，源代码先翻译成中间代码，再由解释器对中间代码进行解释运行。每次运行的时候都要逐行解释一遍，有良好的平台兼容性。如JavaScript、Python、PHP

> 动态类型语言 vs 静态类型语言
动态类型语言，是指数据类型的检查是在运行时做的。用动态类型语言编程时，不用给变量指定数据类型，该语言会在你第一次赋值给变量时，在内部记录数据类型。
静态类型语言，是指数据类型的检查是在运行前（如编译阶段）做的。

* JavaScript语言的历史
1995年5月，Brendan Eich只用了10天，就设计完成了这种语言的第一版。

1996年，样式表标准CSS第一版发布。
1997年，DHTML（Dynamic HTML，动态HTML）发布，允许动态改变网页内容。这标志着DOM模式（Document Object Model，文档对象模型）正式应用。

1997年7月，ECMAScript 1.0。
1998年6月，ECMAScript 2.0。格式修正，以使得其形式与ISO/IEC16262国际标准一致。
1999年12月，ECMAScript 3.0。强大的正则表达式，更好的文字链处理，新的控制指令，异常处理，错误定义更加明确，数输出的格式化及其它改变。

2001年，微软公司时隔5年之后，发布了IE浏览器的下一个版本Internet Explorer 6。
2002年，Mozilla项目发布了它的浏览器的第一版，后来起名为Firefox。
2003年，苹果公司发布了Safari浏览器的第一版。
2005年，Ajax方法（Asynchronous JavaScript and XML）正式诞生。Google Maps项目大量采用该方法。促成了Web 2.0时代的来临。
2005年，Apache基金会发布了CouchDB数据库。基于JSON格式的数据库，可以用JavaScript函数定义视图和索引，标识着NoSQL类型的数据库诞生。
2006年，jQuery函数库诞生，作者为John Resig。
2006年，微软公司发布IE 7，标志重新开始启动浏览器的开发。
2008年，V8编译器诞生。这是Google公司为Chrome浏览器而开发的，它的特点是让JavaScript的运行变得非常快。V8是开源的，拓展了JavaScript的应用领域。
2009年，Node.js项目诞生，创始人为Ryan Dahl，它标志着JavaScript可以用于服务器端编程，从此网站的前端和后端可以使用同一种语言开发。

2009年12月，ECMAScript 5.0。
2011年6月，ECMAscript 5.1，并且成为ISO国际标准（ISO/IEC 16262:2011）。到了2012年底，所有主要浏览器都支持ECMAScript 5.1版的全部功能。

2012年，SPA单页面应用程序框架开始崛起，AngularJS项目和Ember项目都发布了1.0版本。
2012年，微软发布TypeScript语言。该语言被设计成JavaScript的超集，这意味着所有JavaScipt程序，都可以不经修改地在TypeScript中运行。
2013年5月，Facebook发布UI框架库React，引入了新的JSX语法，使得UI层可以用组件开发。
2015年3月，Facebook公司发布了React Native项目，将React框架移植到了手机端，可以用来开发手机原生App。
2015年4月，Angular框架宣布，2.0版将基于微软公司的TypeScript语言开发，这等于为JavaScript语言引入了强类型。

2015年6月17日，ECMAScript 6，更名为 ECMAScript 2015。新增let、const、class、modules、arrow functions、rest argument、binary data、promises等等。这个标准从提出到批准，历时10年，而JavaScript语言从诞生至今也已经20年了。
2016年06月，ECMAScript 2016。完善ES6规范，两个新的功能：求幂运算符（*）和array.prototype.includes方法。
2017年06月：ECMAScript 2017。增加新的功能，如并发、原子操作、Object.values/Object.entries、字符串填充、promises、await/asyn等等。
2017年11月，所有主流浏览器全部支持 WebAssembly，这意味着任何语言都可以编译成 JavaScript，在浏览器运行。

