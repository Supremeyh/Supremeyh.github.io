---
title: 你问我答 vue
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

* 简单描述每个周期具体适合哪些场景
生命周期：Vue 实例从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期，各个阶段有相对应的事件钩子

beforeCreate	实例初始化之后，this指向创建的实例，数据观测（data observer）和event/watcher配置尚未完成，不能访问到data、computed、watch、methods上的方法和数据。 $el和数据对象data都为undefined。 常用于初始化非响应式变量，loading事件

created	实例创建完成，完成数据观测(data observer)，属性和方法的运算， watch/event 事件回调。可访问data、computed、watch、methods上的方法和数据。 还未挂载到DOM，不能访问到$el属性，$ref属性内容为空数组。 数据对象data有了，$el还没有。 常用于简单的ajax请求，获取初始化数据对实例进行初始化预处理，结束loading事件

beforeMount	在挂载开始之前被调用，beforeMount之前，会找到对应的template，并编译成render函数。 vue实例的$el和data都初始化了，但还是挂载之前为虚拟的dom节点，data.message还未替换。
mounted	实例挂载到DOM上，此时可以通过DOM API获取到DOM节点，$ref属性可以访问，在这个阶段改变data上的值，相关的computed属性可以立刻更新	。vue实例挂载完成，data.message成功渲染。 常用于获取VNode信息和操作，ajax请求
beforeupdate	响应式数据更新时调用，发生在虚拟DOM打补丁之前。 适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器
updated	虚拟 DOM 重新渲染和打补丁之后调用，组件DOM已经更新，可执行依赖于DOM的操作。 避免在这个钩子函数中操作数据，可能陷入死循环
beforeDestroy	实例销毁之前调用。这一步，实例仍然完全可用，this仍能获取到实例。 常用于销毁定时器、解绑全局事件、销毁插件对象等操作
destroyed	实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁	–
nextTick : 更新数据后立即操作dom

created阶段的ajax请求与mounted请求的区别：前者页面视图未出现，如果请求信息过多，页面会长时间处于白屏状态
mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick

* 封装 vue 组件的过程
使用Vue.extend方法创建一个组件，然后使用Vue.component方法注册组件。子组件需要数据，可以在props中接受定义。而子组件修改好数据后，想把数据传递给父组件。可以采用emit方法。

* 对Vue.js的template编译的理解
答：简而言之，就是先转化成AST树，再得到的render函数返回VNode（Vue的虚拟DOM节点）
详情步骤：
首先，通过compile编译器把template编译成AST语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式），compile是createCompiler的返回值，createCompiler是用以创建编译器的。另外compile还负责合并option。
然后，AST会经过generate（将AST语法树转化成render funtion字符串的过程）得到render函数，render的返回值是VNode，VNode是Vue的虚拟DOM节点，里面有（标签名、子节点、文本等等）


* Vue的双向数据绑定原理是什么
vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty() 来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。
具体步骤：
第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化
第二步：compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
第三步：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情是:
1、在自身实例化时往属性订阅器(dep)里面添加自己
2、自身必须有一个 update() 方法
3、待属性变动 dep.notice() 通知时，能调用自身的 update() 方法，并触发 Compile 中绑定的回调，则功成身退。
第四步：MVVM作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。

* 简述Vue的响应式原理
https://mp.weixin.qq.com/s/gVshLnybS0JG88z1DfoYGg

* 98道经典Vue面试题总结（长期更新）
https://segmentfault.com/a/1190000016351284

* vue 面试试题
https://www.kancloud.cn/hanxuming/vue-iq/728305