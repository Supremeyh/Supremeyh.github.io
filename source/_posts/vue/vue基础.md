---
title: Vue基础
date: 2019-01-22 21:42:48
categories: Vue
tags: ['Vue', 'Vue基础']
---


###  计算属性 computed
对于任何复杂逻辑，都应当使用计算属性。
计算属性缓存 vs 方法： 计算属性是基于它们的依赖进行缓存的，减小性能开销。只在相关依赖发生改变时它们才会重新求值，相比之下，调用方法将总会再次执行函数。
计算属性 vs 侦听属性：侦听器watch是一种更通用的方式来观察和响应实例上的数据变动，当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。 当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch。然而，通常更好的做法是使用计算属性。


###  条件渲染 v-if
* v-if vs v-show。v-if  是“真正”的条件渲染，会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建;也是惰性的，如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。因此，有更高的切换开销。  v-show 始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
* v-if 与 v-for 一起使用。当处于同一节点，v-for 具有比 v-if 更高的优先级，这意味着 v-if 将分别重复运行于每个 v-for 循环中。
为了过滤一个列表中的项目 (比如 v-for="user in users" v-if="user.isActive")，应将 users 替换为一个计算属性 (比如 activeUsers)，让其返回过滤后的列表。 
为了避免渲染本应该被隐藏的列表 (比如 v-for="user in users" v-if="shouldShowUsers")。这种情形下，请将 v-if 移动至容器元素上 (比如 ul, ol)。



### 列表渲染 v-for
* v-for="(value, key, index) in object" 分别为 值，键，索引。在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。
* 数组更新检测
> 变异方法，会触发视图更新：push()、pop()、shift()、unshift()、splice()、sort()、reverse()
> 非变异方法，不会改变原始数组，总是返回一个新数组：filter()、concat()、slice() 。当使用非变异方法时，可以用新数组替换旧数组。
> 由于 JavaScript 的限制，Vue 不能检测以下变动的数组，不能触发状态更新：
1、利用索引直接设置一个项时，
例如：vm.items[indexOfItem] = newValue，
可以使用 Vue.set (即vm.$set): Vue.set(vm.items, indexOfItem, newValue)
也可以使用 Array.prototype.splice: vm.items.splice(indexOfItem, 1, newValue)
2、修改数组的长度时，例如：vm.items.length = newLength
可以使用 vm.items.splice(newLength)
> 由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除：
对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是,
可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性.
也可以使用 Object.assign()两个对象的属性创建一个新的对象 或 _.extend():  vm.Bob = Object.assign({}, vm.Bob, {age:27,sex:'boy'})
* 显示过滤/排序结果
要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据，可以创建返回过滤或排序数组的计算属性。v-for="n in evenNumbers"；
在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法：v-for="n in even(numbers)" .
* 一段取值范围的 v-for
v-for 也可以取整数。在这种情况下，它将重复多次模板。v-for="n in 10" 为1,2...10

* vue选中某一项
```
<div v-for= "(item, index) in List">
  <p :class="{'current': currentChecked===index}" @click="currentChecked=index"></p>
</div>

```

### 事件处理 event
* 在内联语句处理器中访问原始的 DOM 事件,可以用特殊变量 $event 把它传入方法 @click="showMsg($event)"
* 事件修饰符
.stop 阻止单击事件继续传播，相当于event.stopPropagation() 阻止事件冒泡
.prevent 取消默认行为，相当于event.preventDefault(). 如@submit.prevent 提交事件不再重载页面
.capture 使用事件捕获模式, 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理
.self 只当在 event.target 是当前元素自身时触发处理函数,即事件不是从内部子元素触发的
.once：只触发一次
.passive 不阻止事件的默认行为,用于scoll、touchmove性能优化,
修饰符可以串联: @click.stop.prevent.
使用修饰符时，顺序很重要,相应的代码会以同样的顺序产生:
@click.prevent.self 会阻止所有的点击
@click.self.prevent 只会阻止对元素自身的点击
@scroll.passive 滚动事件的默认行为 (即滚动行为) 将会立即触发,而不会等待 onScroll 完成。该修饰符尤其能够提升移动端的性能。
.passive和.prevent 不能一起使用。.passive 会告诉浏览器你不想阻止事件的默认行为。.prevent 将会被忽略，同时浏览器可能会一个警告。
v-on 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 v-on:myEvent 将会变成 v-on:myevent——导致 myEvent 不可能被监听到。因此，推荐始终使用 kebab-case 的事件名。

* 按键修饰符
@keyup.13  在 keyCode 是 13 时调用
按键别名: .enter、.tab、.delete (捕获“删除”和“退格”键)、.esc、.space、.up、.down、.left、.right。
通过全局 config.keyCodes 对象自定义按键修饰符别名, 如Vue.config.keyCodes.f1 = 112，之后可以使用 @keyup.f1

有一些按键 (.esc 以及所有的方向键) 在 IE9 中有不同的 key 值, 如果你想支持 IE9，它们的内置别名应该是首选。

* 系统修饰键
.ctrl、.alt、.shift、.meta，可以用这些修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。
请注意修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17。

.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。如@click.ctrl.exact有且只有 Ctrl 被按下的时候才触发 

* 鼠标按钮修饰符
.left、.right、.middle，这些修饰符会限制处理函数仅响应特定的鼠标按钮。


### 组件基础 component
* 父子组件间通信
父组件通过 prop 给子组件下发数据，子组件通过$emit触发事件给父组件发送消息，即 prop 向下传递，事件向上传递。props down, events up.

通过事件向父级组件发送消息, 子组件调用$emit方法，向父级组件触发一个事件，如 @click="$emit('boder-larger', 2)； 父组件监听事件这个事件，就像监听一个原生 DOM 事件一样，如 @boder-larger="postBorderSize += $event"。如果这个事件处理函数是一个方法，那么这个值将会作为第一个参数传入这个方法 onLargerFontSize(playload) {...}
```
<div id="app">
    <blog-content 
      v-for="post in posts" 
      :key="post.id" 
      :text="post.text" 
      :style="{border: postBorderSize + 'px solid red',fontSize:postFontSize + 'px'}"
      @boder-larger="postBorderSize += $event"
      @font-larger="onLargerFontSize">
    </blog-text>
  </div>
  <script>
    Vue.component('blog-content', {
      props: ['text'],
      template: 
      `<div>
        <p>{{text}}</p>
        <button @click="$emit('boder-larger', 2)">Border Larger</button>
        <button @click="$emit('font-larger', 5)">Font Larger</button>
      </div>`
    })

    var vm = new Vue({
      data: {
        posts: [
          {id: 1, text: 'cnn'},
          {id: 2, text: 'bbc'},
          {id: 3, text: 'npr'},
        ],
        postBorderSize: 1,
        postFontSize: 20
      },
      methods:{
        onLargerFontSize(playload){
          this.postFontSize += playload
        }
      }
    }).$mount('#app)
  </script>

```

* 在组件上使用-v-model
```
// 以下代码等价
<input type="text" v-model='searchSth'>
<input type="text" :value="searchSth" @input="searchSth=$event.target.value">

// 在父组件中
<custom-input :value="searchSth" @input="searchSth=$event"></custom-input>

// 子组件input要点：将其 value 特性绑定到一个名叫value 的 prop 上； 在其input 事件被触发时，将新的值通过自定义的input事件抛出.
Vue.component('custom-input',{
  props: ['value'],
  template: `<input :value="value"  @input="$emit('input', $event.target.value)" />`
})

// 之后，就可以使用v-model了
<custom-input v-model="searchSth"></custom-input>

```

* 通过插槽 slot 分发内容
```
// 和 HTML 元素一样，我们经常需要向一个组件传递内容，像这样：
<my-component> welcome my blog </my-component>

Vue.component('my-component',{
  props: ['value'],
  template: 
  `<div>
    <p> hey man </p>
    <slot></slot>
  </div>`
})

```

* 动态组件
在不同组件之间进行动态切换,可以通过 Vue 的 component 元素加一个特殊的 is 特性来实现：
```
// 组件会在 `currentTab` 改变时改变 
<component v-bind:is="currentTab"></component>

```

* 解析 DOM 模板时的注意事项
有些HTML元素，诸如 ul、ol、table 和 select，对于内部子元素有严格限制的。而有些元素诸如 li、tr 和 option，也只能出现在其它某些特定的元素内部。如
```
<table>
  <my-post></my-post>
</table>
```
这个自定义组件 my-post 会被作为无效的内容提升到外部，并导致最终渲染结果出错。可以使用特殊的 is 特性：
```
<table>
  <tr is="my-post"></tr>
</table>
```
但如果我们从以下来源使用模板的话，这条限制是不存在的：
字符串 (例如：template: '...')、单文件组件 (.vue)、<script type="text/x-template">


* 组件名大小写
在单文件组件和字符串模板中组件名应该总是 PascalCase 的，但是在 DOM 模板中总是 kebab-case 的。

使用 kebab-case：当使用 kebab-case (短横线分隔命名) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case，例如 my-component-name。

使用 PascalCase: 当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 my-component-name 和 MyComponentName 都是可接受的。但直接在 DOM (即非字符串的模板) 中使用时,由于 HTML 是大小写不敏感的,只有 kebab-case 是有效的。


* Prop 名大小写
在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。 根据语言规范，在 JavaScript 中更自然的是 camelCase，而在 HTML 中则是 kebab-case。

HTML 中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名，但如果使用字符串模板，那么这个限制就不存在了。


包含该 prop 没有值的情况在内，都意味着true
```
<blog-post is-published></blog-post>
```

即便数组是静态的，我们仍然需要 v-bind 来告诉 Vue, 这是一个 JavaScript 表达式而不是一个字符串
```
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>
```

将一个对象的所有属性都作为 prop 传入，可以使用不带参数的 v-bind (取代 v-bind:prop-name)
```
// 以下两种等价
<blog-post v-bind="post"></blog-post> 

<blog-post v-bind:id="post.id" v-bind:title="post.title"></blog-post>
```

* 禁用特性继承 
默认情况下父作用域的不被认作 props 的特性绑定 (attribute bindings) 将会“回退”且作为普通的 HTML 特性应用在子组件的根元素上。
inheritAttrs 设置为false控制去掉vue对非prop特性默认行为, $attrs 存储非prop特性。


* 自定义事件
不同于组件和 prop，事件名不会被用作一个 JavaScript 变量名或属性名，不存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称。
因此事件名所以就没有理由使用 camelCase 或 PascalCase 了，并且 v-on 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 v-on:myEvent 将会变成 v-on:myevent，导致 myEvent 不可能被监听到。
因此，我们推荐你始终使用 kebab-case 的事件名。

* keep-alive
包裹动态组件时，主要用于保留组件状态或避免重新渲染，缓存不活动的组件实例，而不是销毁它们。和 transition 相似，keep-alive>是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中。
当组件在 keep-alive内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。
用在其一个直属的子组件被开关的情形。如果你在其中有 v-for 则不会工作。
不会在函数式组件中正常工作，因为它们没有缓存实例。
props: include、exclude、max

* 处理边界情况
访问根实例: this.$root
访问父级组件实例: this.$parent
访问子组件实例或子元素: this.$refs，在组件渲染完成之后生效，并且它们不是响应式的
依赖注入： provide 选项允许指定想要提供给后代组件的数据/方法；然后在任何后代组件里，都可以使用 inject 选项来接收指定的想要添加在这个实例上的属性。
这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。

* 过渡 动画
过渡的类名：v-enter、v-enter-active、v-enter-to、v-leave、v-leave-active、v-leave-to。 v-enter-active 和 v-leave-active 可以控制过渡的过程时间，延迟和曲线函数。

自定义过渡的类名：enter-class、enter-active-class、enter-to-class、leave-class、leave-active-class、leave-to-class。优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 Animate.css 结合使用十分有用。

显性的过渡持续时间：用 transition 组件上的 duration 属性定制一个显性的过渡持续时间 (以毫秒计)，如transition :duration="{ enter: 500, leave: 800 }"

在属性中声明JavaScript-钩子：
```
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>


methods: {
  beforeEnter: function (el) {
    // ...
  },
  // 当与 CSS 结合使用时 回调函数 done 是可选的
  enter: function (el, done) {
    // ...
    done()
  }

}
```
当只用 JavaScript 过渡的时候，在 enter 和 leave 中必须使用 done 进行回调。否则，它们将被同步调用，过渡会立即完成。
推荐对于仅使用 JavaScript 过渡的元素添加 v-bind:css="false"，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响。

初始渲染的过渡：通过 appear 特性设置节点在初始渲染的过渡

过渡模式：transition name="fade" mode="out-in" 
in-out：新元素先进行过渡，完成之后当前元素过渡离开。 out-in：当前元素先进行过渡，完成之后新元素过渡进入。 

多个组件的过渡：不需要使用 key 特性。相反，我们只需要使用动态组件 component v-bind:is="view"

列表过渡：使用 transition-group 组件，不同于 transition，它会以一个真实元素呈现，默认为一个 span。你也可以通过 tag 特性更换为其他元素。过渡模式不可用，因为我们不再相互切换特有的元素。内部元素 总是需要 提供唯一的 key 属性值。 不仅可以进入和离开动画，还可以改变定位。要使用这个新功能只需了解新增的 v-move 特性，它会在元素的改变定位的过程中应用。可以通过 name 属性来自定义前缀，也可以通过 move-class 属性手动设置，v-move 对于设置过渡的切换时机和过渡曲线非常有用。


* 状态过渡
Vue 的过渡系统提供了非常多简单的方法设置进入、离开和列表的动效。那么对于数据元素本身的动效呢，比如：数字和运算、颜色的显示、SVG 节点的位置、元素的大小和其他的属性。这些数据要么本身就以数值形式存储，要么可以转换为数值。有了这些数值后，我们就可以结合 Vue 的响应式和组件系统，使用第三方库来实现切换元素的过渡状态。


### 可复用性 & 组合
* 混入 mixins 
一种分发 Vue 组件中可复用功能的非常灵活的方式。混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项。
同名钩子函数将混合为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用。
```
let vm = new Vue({
  data:{
    msg: 'hi',
  },
  mixins: [myMixin]
}).$mount('#app')
```

Vue.mixin({}) 也可以全局注册混入对象。一旦使用全局混入对象，将会影响到 所有 之后创建的 Vue 实例。使用恰当时，可以为自定义对象注入处理逻辑。


* 自定义指令 directive
除了核心功能默认内置的指令 (v-model 和 v-show)，Vue 也允许注册自定义指令。代码复用和抽象的主要形式是组件。然而，有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令。举个聚焦输入框的例子，如下：
```
// 全局指令
Vue.directive('focus', {
  inserted: function (el) {
    el.focus()
  }
})

//局部指令
directives:{
  focus: {
    inserted: function (el) {
      el.focus()
    }
  }
}

<input v-focus type="text">
```
钩子函数，一个指令定义对象可以提供如下几个钩子函数 (均为可选)：
bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。
componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。
unbind：只调用一次，指令与元素解绑时调用。

钩子函数参数  除了 el 之外，其它参数都应该是只读的，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 dataset 来进行。
el：指令所绑定的元素，可以用来直接操作 DOM 。
vnode：Vue 编译生成的虚拟节点。移步 VNode API 来了解更多详情。
oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。
binding：一个对象，包含以下属性：
name：指令名，不包括 v- 前缀。
value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。
oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
expression：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。
arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。
modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。


### 渲染函数 & JSX
Vue 推荐在绝大多数情况下使用 template 来创建HTML。然而在一些场景中，需要 JavaScript 的完全编程的能力，这时你可以用 render 函数，它比 template 更接近编译器。
```
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h'+ this.level,
      this.$slots.default
    )
  },
  props:{
    level:{
      type: Number,
      required: true
    }
  }
})
```

* 虚拟 DOM
Vue 通过建立一个虚拟 DOM 对真实 DOM 发生的变化保持追踪。
createElement 到底会返回什么呢？其实不是一个实际的 DOM 元素。它更准确的名字可能是 createNodeDescription，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，及其子节点。我们把这样的节点描述为“虚拟节点 (Virtual Node)”，也常简写它为“VNode”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。

* createElement 参数
```
// @returns {VNode}
createElement(
  // {String | Object | Function}  一个 HTML 标签字符串，组件选项对象，或者解析上述任何一种的一个 async 异步函数。必需参数。
  'div',

  // {Object}  一个包含模板相关属性的数据对象，你可以在 template 中使用这些特性。可选参数。
  {
    // (详情见下一节)
  },

  // {String | Array}  子虚拟节点 (VNodes)，由 `createElement()` 构建而成，也可以使用字符串来生成“文本虚拟节点”。可选参数。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```
深入 data 对象
```
{
  // 和`v-bind:class`一样的 API，接收一个字符串、对象或字符串和对象组成的数组
  'class': {
    foo: true,
    bar: false
  },
  // 和`v-bind:style`一样的 API，接收一个字符串、对象或对象组成的数组
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 普通的 HTML 特性
  attrs: {
    id: 'foo'
  },
  // 组件 props
  props: {
    myProp: 'bar'
  },
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
  // 事件监听器基于 `on`，所以不再支持如 `v-on:keyup.enter` 修饰器，需要手动匹配 keyCode。
  on: {
    click: this.clickHandler
  },
  // 仅用于组件，用于监听原生事件，而不是组件内部使用，`vm.$emit` 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`赋值，因为 Vue 已经自动为你进行了同步。
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // 作用域插槽格式 { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // 如果组件是其他组件的子组件，需为插槽指定名称
  slot: 'name-of-slot',
  // 其他特殊顶层属性
  key: 'myKey',
  ref: 'myRef',
  // 如果你在渲染函数中向多个元素都应用了相同的 ref 名，那么 `$refs.myRef` 会变成一个数组。
  refInFor: true
}
```

插槽
通过 this.$slots 访问静态插槽的内容，得到的是一个 VNodes 数组：
```
render: function (createElement) {
  // `<div><slot></slot></div>`
  return createElement('div', this.$slots.default)
}

<anchored-heading message="happy 2019">
  this is anchored heading
</anchored-heading>
```

通过 this.$scopedSlots 访问作用域插槽，得到的是一个返回 VNodes 的函数：
```
props: ['message'],
render: function (createElement) {
  // `<div><slot :text="message"></slot></div>`
  return createElement('div', [
    this.$scopedSlots.default({
      text: this.message
    })
  ])
}


<anchored-heading message="happy 2019">
  <template slot-scope="props">
    {{props}}
  </template>
</anchored-heading>
```


* 函数式组件
functional 无状态 (没有响应式数据)，无实例 (没有 this 上下文)。没有管理或者监听任何传递给他的状态，也没有生命周期方法。它只是一个接收参数的函数。
```
Vue.component('my-functional-button', {
  functional: true,
  // Props 可选
  props: { // ... },
  // 为了弥补缺少的实例，提供第二个参数作为上下文
  render: function (createElement, context) {
    // 完全透明的传入任何特性、事件监听器、子结点等。
    return createElement('button', context.data, context.children)
  }
})
```


### 插件
插件通常会为 Vue 添加全局功能。插件的范围没有限制——一般有下面几种：
种类：添加全局方法或者属性、添加全局资源：指令/过滤器/过渡等、通过全局 mixin 方法添加一些组件选项、一个库，提供自己的 API，同时提供上面提到的一个或多个功能
使用插件：Vue.use(MyPlugin, { someOption: true })
开发插件：Vue.js 的插件应该有一个公开方法 install。这个方法的第一个参数是 Vue 构造器，第二个参数是一个可选的选项对象：
```
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }

  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })

  // 3. 注入组件
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```

### 过滤器
用于一些常见的文本格式化。过滤器可以用在两个地方：双花括号插值和 v-bind 表达式。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示：
过滤器可以串联 {{ message | filterA | filterB }}
过滤器是 JavaScript 函数，因此可以接收参数：{{ message | filterA('arg1', arg2) }}  ，此处接收三个参数，message 的值作为第一个参数，普通字符串 'arg1' 作为第二个参数，表达式 arg2 的值作为第三个参数。


### 路由
* SPA缺点：不利于SEO; 浏览器前进后退会重新发送请求，没有合理利用缓存;  无法记住之前滚动的位置;


### 深入响应式原理
Vue 最独特的特性之一，是其非侵入性的响应式系统。数据模型仅仅是普通的 JavaScript 对象。而当你修改它们时，视图会进行更新。这使得状态管理非常简单直接。

* 如何追踪变化
当你把一个普通的 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty(obj, prop, descriptor)把这些属性全部转为 getter/setter。Object.defineProperty 是 ES5 中一个无法 shim 的特性，这也就是为什么 Vue 不支持 IE8 以及更低版本浏览器。

每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。

* 声明响应式属性：
受现代 JavaScript 的限制，Vue 不能检测到对象属性的添加或删除。由于 Vue 会在初始化实例时对属性执行 getter/setter 转化过程，所以属性必须在 data 对象上存在才能让 Vue 转换它，这样才能让它是响应的。
由于 Vue 不允许动态添加根级响应式属性，所以你必须在初始化实例前声明根级响应式属性，哪怕只是一个空值。

然而它可以使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上，或者有时你想向一个已有对象添加多个属性，例如使用 Object.assign() 或 _.extend() 方法来添加属性。this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })


* 异步更新队列
Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MessageChannel，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。

在组件内使用 vm.$nextTick() 实例方法特别方便，因为它不需要全局 Vue ，并且回调函数中的 this 将自动绑定到当前的 Vue 实例上。

```
methods: {
  updateMessage: function () {
    this.message = '更新完成'
    console.log(this.$el.textContent) // => '没有更新'
    this.$nextTick(function () {
      console.log(this.$el.textContent) // => '更新完成'
    })
  }
}
```
因为 $nextTick() 返回一个 Promise 对象，所以你可以使用新的 ES2016 async/await 语法完成相同的事情：
```
methods: {
  async updateMessage: function () {
    this.message = 'updated'
    console.log(this.$el.textContent) // => '未更新'
    await this.$nextTick()
    console.log(this.$el.textContent) // => '已更新'
  }
}
```