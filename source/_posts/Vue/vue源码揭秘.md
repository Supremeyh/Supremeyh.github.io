---
title: vue源码揭秘
comments: true
date: 2019-02-17 22:52:59
categories: Vue
tags: ['Vue', 'Vue基础']
---
> Reactive, component-oriented view layer for modern web interfaces.

### Flow
FlowFlow是facebook 出品的JavaScript 静态类型检查工具。Vue.js 的源码利用了Flow 做了静态类型检查，所以了解Flow 有助于我们阅读源码，并且这种静态类型检查的方式非常有利于大型项目源码的开发和维护。

Vue.js 在做2.0 重构的时候，在ES2015 的基础上，除了ESLint 保证代码风格之外，也引入了Flow 做静态类型检查。之所以选择Flow，主要是因为Babel 和ESLint 都有对应的Flow 插件以支持语法，可以完全沿用现有的构建配置，非常小成本的改动就可以拥有静态类型检查的能力。

Flow 在 Vue.js源码中的应用有时候我们想引用第三方库，或者自定义一些类型，但Flow 并不认识，因此检查的时候会报错。为了解决这类问题，Flow 提出了一个libdef的概念，可以用来识别这些第三方库或者是自定义类型，而Vue.js 也利用了这一特性。在Vue.js 的主目录下有.flowconfig文件，它是Flow 的配置文件，感兴趣的同学可以看官方文档。这其中的[libs]部分用来描述包含指定库定义的目录，默认是名为flow-typed的目录。这里[libs]配置的是flow，表示指定的库定义都在flow文件夹内。


### Vue.js 源码目录设计
Vue.js 的源码都在 src 目录下，其目录结构如下。
```
src
├── compiler        # 编译相关。包括把模板解析成 ast 语法树，ast 语法树优化，代码生成等功能。
├── core            # 核心代码。 Vue.js 的灵魂。包括内置组件、全局 API 封装，Vue 实例化、观察者、虚拟 DOM、工具函数等等。
├── platforms       # 不同平台的支持。 Vue.js 的入口，2个目录代表2个主要入口，分别打包成运行在 web 上和配合weex跑在native客户端上。
├── server          # 服务端渲染。跑在服务端的 Node.js，把组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器。
├── sfc             # .vue 文件解析。把.vue文件内容解析成一个 JavaScript 的对象。
├── shared          # 共享代码。定义被浏览器和服务端的 Vue.js 所共享的工具方法。
```
功能模块拆分的非常清楚，相关的逻辑放在一个独立的目录下维护，并且把复用的代码也抽成一个独立目录。这样的目录设计让代码的阅读性和可维护性都变强。


### Vue.js 源码构建
* Runtime Only VS Runtime + Compiler
Runtime Only: 借助如 webpack 的 vue-loader 工具把 .vue 文件编译成 JavaScript，因为是在编译阶段做的，所以它只包含运行时的 Vue.js 代码，因此代码体积也会更轻量。
Runtime + Compiler: 如果没有对代码做预编译，但又使用了 Vue 的 template 属性并传入一个字符串，则需要在客户端编译模板。这个编译过程对性能会有一定损耗。

### 从入口开始
以Runtime+compiler CommonJS build (CommonJS)为例，入口是 src/platforms/web/entry-runtime-with-compiler.js。 最终在源头src/core/instance/index.js找到Vue的定义，看到了Vue的庐山真面目，它实际上就是一个用 Function 实现的类，我们只能通过 new Vue 去实例化它。
```
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)

export default Vue
}
```
为何 Vue 不用 ES6 的 Class 去实现呢？我们往后看这里有很多如 initMixin 的函数调用，并把 Vue 当参数传入，它们的功能都是给 Vue 的 prototype 上扩展一些方法，Vue 按功能把这些扩展分散到多个模块中去实现，而不是在一个模块里实现所有，这种方式是用 Class 难以实现的。这么做的好处是非常方便代码的维护和管理，这种编程技巧也非常值得我们去学习。

initGlobalAPI
在src/core/index.js发现，Vue.js 在整个初始化过程中，除了给它的原型 prototype 上扩展方法，还会给 Vue 这个对象本身扩展全局的静态方法，Vue 官网中关于全局 API 都可以在这里找到。它的定义在 src/core/global-api/index.js 中。
```
export function initGlobalAPI (Vue: GlobalAPI) {
  // config
  const configDef = {}
  configDef.get = () => config
  if (process.env.NODE_ENV !== 'production') {
    configDef.set = () => {
      warn(
        'Do not replace the Vue.config object, set individual fields instead.'
      )
    }
  }
  Object.defineProperty(Vue, 'config', configDef)

   Vue.util = { warn, extend, mergeOptions, defineReactive }

  Vue.set = set
  Vue.delete = del
  Vue.nextTick = nextTick
  // ...
}
  ```
这里就是在 Vue 上扩展的一些全局方法的定义，Vue 官网中关于全局 API 都可以在这里找到，这里不会介绍细节，会在之后的章节我们具体介绍到某个 API 的时候会详细介绍。有一点要注意的是，Vue.util 暴露的方法最好不要依赖，因为它可能经常会发生变化，是不稳定的。

至此，直观的认识到，Vue本质上就是一个用 Function 实现的 Class，然后它的原型 prototype 以及它本身都扩展了一系列的方法和属性。