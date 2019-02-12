---
title: vue API
comments: true
date: 2019-02-12 14:46:02
categories: ['Vue', 'Vue基础']
tags: ['Vue', 'Vue基础']
---

### 全局配置
Vue.config 是一个对象，包含 Vue 的全局配置。
silent： Vue.config.silent = true，取消 Vue 所有的日志与警告。
devtools：Vue.config.devtools = true，配置是否允许 vue-devtools 检查代码。开发版本默认为 true，生产版本默认为 false。生产版本设为 true 可以启用检查。
productionTip：设置为 false 以阻止 vue 在启动时生成生产提示。
errorHandler：Vue.config.errorHandler = function (err, vm, info) {}，指定组件的渲染和观察期间未捕获错误的处理函数。
warnHandler：Vue.config.warnHandler = function (msg, vm, trace) {}，为 Vue 的运行时警告赋予一个自定义处理函数。只在开发者环境下生效。
ignoredElements：Vue.config.ignoredElements = ['my-custom-web-component',/^ion-/]，忽略在 Vue 之外的自定义元素 
keyCodes：Vue.config.keyCodes = {f1: 112,"media-play-pause": 179,up: [38, 87]}，给 v-on 自定义键位别名。 camelCase 不可用,使用kebab-case 且用双引号括起来
performance: 在浏览器开发工具的性能/时间线面板中启用对组件初始化、编译、渲染和打补丁的性能追踪。只适用于开发模式和支持 performance.mark API 的浏览器上。


### 全局 API


