---
title: 面试题 HTML和CSS
comments: true
date: 2019-03-22 10:07:06
categories: Review
tags: ['Review', 'HTML', 'CSS']
---


1. 什么是盒子模型
在网页中，把HTML中的元素看做是一个矩形的盒子（盛装内容的的容器），每个容器都是由元素内容content、内边距（padding）、边框（border）和外边距（margin）组成。4个部分一起构成了css中元素的盒模型。

两种盒子模型
w3c的盒子模型：padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 (Element width = width + border + padding ) ，此属性表现为标准模式下的盒模型。 box-sizing:content-box;

IE的盒子模型：padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 (Element width = width ) 。 box-sizing:border-box;

W3C盒模型与IE盒模型的区别就是对宽高的定义不同。W3C认为，宽高是内容区的宽度（只包含节点显示的具体内容）。IE认为，宽高是显示效果的实际效果（包含节点的全部内容）

2. 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？
行内元素：a、b、span、img、input、strong、select、label、em、button、textarea
块级元素：div、ul、li、dl、dt、dd、p、h1-h6、blockquote
空元素：即系没有内容的HTML元素，例如：br、meta、hr、link、input、img

3. href与src的区别
src，source的简写，表示“源”，是引用一个资源，用来整体替换自身的内容。在 img、script、iframe 等元素上使用。
href，Hypertext Reference的缩写，表示“超链接”，可叠加，目的不是为了引用一个资源，而是为了建立联系，让当前标签能够链接到目标地址上。在 link和a 等元素上使用。

4. link与@import之间的区别
两者都是外部引用CSS的方式，但是存在一定的区别：
区别1：link除了引用样式文件，还可以引用图片等资源文件，而import只引用CSS样式文件
区别2：link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
区别3：link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
区别4：ink支持使用Javascript控制DOM去改变样式；而@import不支持。

5. 浏览器的内核(Rendering Engine渲染引擎)
IE: Trident
Firefox：Gecko
Safari：Webkit内核
Opera：以前是Presto内核，现改用Blink内核
Chrome：Blink(基于webkit，Google与Opera Software共同开发)
