---
title: 面试题-Vue
comments: true
date: 2019-07-25 23:13:43
categories: Review
tags: tags: ['Review', 'Vue']
---

### vue-cli 
1. 构建的 vue-cli 工程都到了哪些技术，它们的作用分别是什么？
1、vue.js：vue-cli工程的核心，主要特点是 双向数据绑定 和 组件系统。
2、vue-router：vue官方推荐使用的路由框架。
3、vuex：专为 Vue.js 应用项目开发的状态管理器，主要用于维护vue组件间共用的一些 变量 和 方法。
4、axios（ 或者 fetch 、ajax ）：用于发起 GET 、或 POST 等 http请求，基于 Promise 设计。
5、vux等：一个专为vue设计的移动端UI组件库。
6、创建一个emit.js文件，用于vue事件机制的管理。
7、webpack：模块加载和vue-cli工程打包器。

2. vue-cli 工程常用的 npm 命令有哪些
npm install 
npm run dev 
npm run build 
npm run build --report  用于查看 vue-cli 生产环境部署资源文件大小的 npm命令

3. 对于MVVM的理解
MVVM 是 Model-View-ViewModel 的缩写。
Model代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑。
View 代表UI 组件，它负责将数据模型转化成UI 展现出来。
ViewModel 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步View 和 Model的对象，连接Model和View。
在MVVM架构下，View 和 Model 通过ViewModel进行交互，Model 和 ViewModel 之间的交互是双向的， 因此View 数据的变化会同步到Model中，而Model 数据的变化也会立即反应到View 上。因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

4. Vue的生命周期
Vue 实例从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期，各个阶段有相对应的事件钩子。

beforeCreate 初始化事件，进行数据的观测，this指向创建的实例。el、data、message 都为 undefined。常用于初始化非响应式变量
created  完成数据观测，属性和方法的运算。el 为 undefined， data 已被初始化，message有值，数据已经和data属性进行绑定。用于简单的ajax请求，页面的初始化。
beforeMount 会找到对应的template，并编译成render函数，相关的render函数首次被调用，编译模板，把data里面的数据和模板生成html。
mounted 编译好的html内容替换el属性指向的DOM对象，模板中的html渲染到html页面中。$ref属性可以访问。常用于获取VNode信息和操作，ajax请求
beforeUpdate  响应式数据更新时调用，发生在虚拟DOM打补丁之前。适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器。
updated  组件DOM已经更新，可以执行依赖于DOM的操作。但在大多数情况下，应该避免在此期间更改状态，可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用
beforeDestroy  实例销毁之前，实例仍然完全可用。常用于销毁定时器、解绑全局事件、销毁插件对象等操作
destroyed 实例销毁，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

created阶段的ajax请求与mounted请求的区别：前者页面视图未出现，如果请求信息过多，页面会长时间处于白屏状态
mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick
