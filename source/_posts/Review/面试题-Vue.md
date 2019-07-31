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
