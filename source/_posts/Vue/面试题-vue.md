---
title: 面试题 vue
comments: true
date: 2019-02-26 14:31:54
categories: Vue
tags: ['Vue', 'Review']
---

* 谈谈你对MVVM开发模式的理解
MVVM分为Model、View、ViewModel三者。
Model：代表数据模型，数据和业务逻辑都在Model层中定义；
View：代表UI视图，负责数据的展示；
ViewModel：负责监听Model中数据的改变并且控制视图的更新，处理用户交互操作；
Model和View并无直接关联，而是通过ViewModel来进行联系的，Model和ViewModel之间有着双向数据绑定的联系。因此当Model中的数据改变时会触发View层的刷新，View中由于用户交互操作而改变的数据也会在Model中同步。这种模式实现了Model和View的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作dom。

* 简述Vue的响应式原理
https://mp.weixin.qq.com/s/gVshLnybS0JG88z1DfoYGg
