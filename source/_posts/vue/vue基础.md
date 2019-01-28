---
title: Vue基础
date: 2019-01-22 21:42:48
categories: Vue
tags: ['Vue', 'Vue基础']
---
## 父子组件间通信
> 父组件通过 prop 给子组件下发数据，子组件通过$emit触发事件给父组件发送消息，即 prop 向下传递，事件向上传递。props down, events up.

## 路由
* SPA缺点：不利于SEO; 
* 浏览器前进后退会重新发送请求，没有合理利用缓存; 
* 无法记住之前滚动的位置;


### computed 计算属性
对于任何复杂逻辑，都应当使用计算属性。
计算属性缓存 vs 方法： 计算属性是基于它们的依赖进行缓存的，减小性能开销。只在相关依赖发生改变时它们才会重新求值，相比之下，调用方法将总会再次执行函数。
计算属性 vs 侦听属性：侦听器watch是一种更通用的方式来观察和响应实例上的数据变动，当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。 当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch。然而，通常更好的做法是使用计算属性。


### v-if 条件渲染
* v-if vs v-show。v-if  是“真正”的条件渲染，会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建;也是惰性的，如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。因此，有更高的切换开销。  v-show 始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
* v-if 与 v-for 一起使用。当处于同一节点，v-for 具有比 v-if 更高的优先级，这意味着 v-if 将分别重复运行于每个 v-for 循环中。
为了过滤一个列表中的项目 (比如 v-for="user in users" v-if="user.isActive")，应将 users 替换为一个计算属性 (比如 activeUsers)，让其返回过滤后的列表。 
为了避免渲染本应该被隐藏的列表 (比如 v-for="user in users" v-if="shouldShowUsers")。这种情形下，请将 v-if 移动至容器元素上 (比如 ul, ol)。



### 列表渲染
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

### 事件处理
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
