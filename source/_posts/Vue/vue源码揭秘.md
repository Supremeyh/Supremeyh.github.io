---
title: vue源码揭秘
comments: true
date: 2019-02-17 22:52:59
categories: Vue
tags: ['Vue', 'Vue基础']
---

* Flow
FlowFlow是facebook 出品的JavaScript 静态类型检查工具。Vue.js 的源码利用了Flow 做了静态类型检查，所以了解Flow 有助于我们阅读源码，并且这种静态类型检查的方式非常有利于大型项目源码的开发和维护。

Vue.js 在做2.0 重构的时候，在ES2015 的基础上，除了ESLint 保证代码风格之外，也引入了Flow 做静态类型检查。之所以选择Flow，主要是因为Babel 和ESLint 都有对应的Flow 插件以支持语法，可以完全沿用现有的构建配置，非常小成本的改动就可以拥有静态类型检查的能力。

Flow 在 Vue.js源码中的应用有时候我们想引用第三方库，或者自定义一些类型，但Flow 并不认识，因此检查的时候会报错。为了解决这类问题，Flow 提出了一个libdef的概念，可以用来识别这些第三方库或者是自定义类型，而Vue.js 也利用了这一特性。在Vue.js 的主目录下有.flowconfig文件，它是Flow 的配置文件，感兴趣的同学可以看官方文档。这其中的[libs]部分用来描述包含指定库定义的目录，默认是名为flow-typed的目录。这里[libs]配置的是flow，表示指定的库定义都在flow文件夹内。
