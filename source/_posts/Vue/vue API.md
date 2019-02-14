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
* Vue.extend( options )
使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。data 选项是特例，需要注意 - 在 Vue.extend() 中它必须是函数
```
// 创建构造器
var Profile = Vue.extend({
  template: '<p>{{alias}}</p>',
  data: function () {
    return {
      alias: 'Heisenberg'
    }
  }
})
// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
```
* Vue.nextTick( [callback, context] )
在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

* Vue.set( target, key, value )
向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新属性，因为 Vue 无法探测普通的新增属性 (比如 this.myObject.newProperty = 'hi')。注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象。

* Vue.delete( target, key )
删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开 Vue 不能检测到属性被删除的限制，但是你应该很少会使用它。目标对象不能是一个 Vue 实例或 Vue 实例的根数据对象。

* Vue.directive( id, [definition] )
注册或获取全局指令。

* Vue.filter( id, [definition] )
注册或获取全局过滤器。

* Vue.component( id, [definition] )
注册或获取全局组件。注册还会自动使用给定的id设置组件的名称

* Vue.use( plugin )
安装 Vue.js 插件。如果插件是一个对象，必须提供 install 方法。如果插件是一个函数，它会被作为 install 方法。install 方法调用时，会将 Vue 作为参数传入。该方法需要在调用 new Vue() 之前被调用。当 install 方法被同一个插件多次调用，插件将只会被安装一次。

* Vue.mixin( mixin )
全局注册一个混入，影响注册之后所有创建的每个 Vue 实例。插件作者可以使用混入，向组件注入自定义的行为。不推荐在应用代码中使用。

* Vue.compile( template )
在 render 函数中编译模板字符串。只在独立构建时有效

* Vue.version
提供字符串形式的 Vue 安装版本号。这对社区的插件和组件来说非常有用，你可以根据不同的版本号采取不同的策略。
var version = Number(Vue.version.split('.')[0])


### 选项 / 数据
* data
Vue 实例的数据对象。Vue 将会递归将 data 的属性转换为 getter/setter，从而让 data 的属性能够响应数据变化。对象必须是纯粹的对象 (含有零个或多个的 key/value 对)：浏览器 API 创建的原生对象，原型上的属性会被忽略。大概来说，data 应该只能是数据 - 不推荐观察拥有状态行为的对象。
以 _ 或 $ 开头的属性 不会 被 Vue 实例代理，因为它们可能和 Vue 内置的属性、API 方法冲突。你可以使用例如 vm.$data._property 的方式访问这些属性。
当一个组件被定义，data 必须声明为返回一个初始数据对象的函数。否则所有的实例将共享引用同一个数据对象。

* methods
methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。

注意，不应该使用箭头函数来定义 method 函数 (例如 plus: () => this.a++)。理由是箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 实例，this.a 将是 undefined。


### 选项 / 生命周期钩子
所有的生命周期钩子自动绑定 this 上下文到实例中，因此你可以访问数据，对属性和方法进行运算。这意味着你不能使用箭头函数来定义一个生命周期方法 (例如 created: () => this.fetchTodos())。这是因为箭头函数绑定了父上下文，因此 this 与你期待的 Vue 实例不同，this.fetchTodos 的行为未定义。


### 实例方法 / 生命周期
* vm.$mount( [elementOrSelector] )
如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 vm.$mount() 手动地挂载一个未挂载的实例。
如果没有提供 elementOrSelector 参数，模板将被渲染为文档之外的的元素，并且你必须使用原生 DOM API 把它插入文档中。
这个方法返回实例自身，因而可以链式调用其它实例方法。
```
var MyComponent = Vue.extend({
  template: '<div>Hello!</div>'
})

// 创建并挂载到 #app (会替换 #app)
new MyComponent().$mount('#app')

// 同上
new MyComponent({ el: '#app' })

// 或者，在文档之外渲染并且随后挂载
var component = new MyComponent().$mount()
document.getElementById('app').appendChild(component.$el)
```

* vm.$nextTick( [callback] )
将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。

如果没有提供回调且在支持 Promise 的环境中，则返回一个 Promise。请注意 Vue 不自带 Promise 的 polyfill，所以如果你的目标浏览器不是原生支持 Promise，你得自行 polyfill。

* vm.$destroy() 
完全销毁一个实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器。触发 beforeDestroy 和 destroyed 的钩子。
在大多数场景中你不应该调用这个方法。最好使用 v-if 和 v-for 指令以数据驱动的方式控制子组件的生命周期。


### 指令
* v-html
更新元素的 innerHTML 。注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译 。如果试图使用 v-html 组合模板，可以重新考虑是否通过使用组件来替代。
在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 XSS 攻击。只在可信内容上使用 v-html，永不用在用户提交的内容上。
在单文件组件里，scoped 的样式不会应用在 v-html 内部，因为那部分 HTML 没有被 Vue 的模板编译器处理。如果你希望针对 v-html 的内容设置带作用域的 CSS，你可以替换为 CSS Modules 或用一个额外的全局 style 元素手动设置类似 BEM 的作用域策略。

* v-pre
跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。

* v-cloak
这个指令保持在元素上直到关联实例结束编译。和 CSS 规则一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。
```
[v-cloak] { display: none } 
```

### 内置的组件
* component
渲染一个“元组件”为动态组件。依 is 的值，来决定哪个组件被渲染。
```
<!-- 动态组件由 vm 实例的属性值 `componentId` 控制 -->
<component :is="componentId"></component>

<!-- 也能够渲染注册过的组件或 prop 传入的组件 -->
<component :is="$options.components.child"></component>
```
* transition-group
transition-group 元素作为多个元素/组件的过渡效果。 渲染一个真实的 DOM 元素。默认渲染 span，可以通过 tag 属性配置哪个元素应该被渲染。
注意，每个 transition-group 的子节点必须有 独立的 key ，动画才能正常工作
```
<transition-group tag="ul" name="slide">
  <li v-for="item in items" :key="item.id">
    {{ item.text }}
  </li>
</transition-group>
```
* keep-alive
主要用于保留组件状态或避免重新渲染。
用在其一个直属的子组件被开关的情形。如果你在其中有 v-for 则不会工作。如果有上述的多个条件性的子元素，keep-alive要求同时只有一个子元素被渲染。
不会在函数式组件中正常工作，因为它们没有缓存实例。
```
<!-- 正则表达式 (使用 `v-bind`) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>
```








