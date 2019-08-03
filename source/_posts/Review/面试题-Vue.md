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

beforeCreate 初始化事件，进行数据的观测，this指向创建的实例。el、data、message 都为 undefined。常用于初始化非响应式变量，loading事件
created  完成数据观测，属性和方法的运算。el 为 undefined， data 已被初始化，message有值，数据已经和data属性进行绑定。用于简单的ajax请求，页面的初始化。
beforeMount 会找到对应的template，并编译成render函数，相关的render函数首次被调用，编译模板，把data里面的数据和模板生成html。
mounted 编译好的html内容替换el属性指向的DOM对象，模板中的html渲染到html页面中。$ref属性可以访问。常用于获取VNode信息和操作，ajax请求
beforeUpdate  响应式数据更新时调用，发生在虚拟DOM打补丁之前。适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器。
updated  组件DOM已经更新，可以执行依赖于DOM的操作。但在大多数情况下，应该避免在此期间更改状态，可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用
beforeDestroy  实例销毁之前，实例仍然完全可用。常用于销毁定时器、解绑全局事件、销毁插件对象等操作
destroyed 实例销毁，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

created阶段的ajax请求与mounted请求的区别：前者页面视图未出现，如果请求信息过多，页面会长时间处于白屏状态
mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick


5. Vue实现数据双向绑定的原理
* 概述
![双向绑定原理](/images/vue-twowaybind-principle.jpg)

采用数据劫持结合发布-订阅模式的方式，通过Object.defineProperty 来劫持各个属性的setter/getter，在数据变动时发布消息给订阅者，触发相应监听回调。

vue的数据双向绑定，将MVVM作为数据绑定的入口，整合Observer，Compile和Watcher三者，通过Observer来监听自己的model的数据变化，通过Compile来解析编译模板指令，最终利用watcher搭起observer和Compile之间的通信桥梁，达到数据变化 —>视图更新；视图交互变化 input —>数据 model 变更双向绑定效果。

* 实现过程
首先要对数据进行劫持监听，所以需要设置一个监听器Observer，用来监听所有属性。如果属性发上变化了，就需要告诉订阅者Watcher看是否需要更新。因为订阅者是有很多个，所以我们需要有一个消息订阅器Dep来专门收集这些订阅者，然后在监听器Observer和订阅者Watcher之间进行统一管理的。接着，我们还需要有一个指令解析器Compile，对每个节点元素进行扫描和解析，将相关指令对应初始化成一个订阅者Watcher，并替换模板数据或者绑定相应的函数，此时当订阅者Watcher接收到相应属性的变化，就会执行对应的更新函数，从而更新视图。因此接下去我们执行以下3个步骤，实现数据的双向绑定：

* 具体实现
1.实现一个监听器 Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。
Observer是一个数据监听器，其实现核心方法就是前文所说的Object.defineProperty()。如果要对所有属性都进行监听的话，那么可以通过递归方法遍历所有属性值，并对其进行Object.defineProperty()处理。如下代码，实现了一个Observer。

监听器的作用就是去监听数据的每一个属性，当我们监听到属性发生变化之后我们需要通知 Watcher 订阅者执行更新函数去更新视图，在这个过程中我们可能会有很多个订阅者 Watcher 所以我们要创建一个容器 Dep 去做一个统一的管理。
```js
function defineReactive(data, key, value) {
  //递归调用，监听所有属性
  observer(value);
  var dep = new Dep();
  Object.defineProperty(data, key, {
    get: function () {
      if (Dep.target) {
        dep.addSub(Dep.target);
      }
      return value;
    },
    set: function (newVal) {
      if (value !== newVal) {
        value = newVal;
        dep.notify(); //通知订阅器
      }
    }
  });
}

function observer(data) {
  if (!data || typeof data !== "object") {
    return;
  }
  Object.keys(data).forEach(key => {
    defineReactive(data, key, data[key]);
  });
}

function Dep () {
  this.subs = [];
}

Dep.prototype = {
  addSub: function(sub) {
    this.subs.push(sub);
  },
  notify: function() {
    this.subs.forEach(function(sub) {
      sub.update();  // 通知所有的订阅者(Watcher)，触发订阅者的相应逻辑处理
    });
  }
};

Dep.target = null;  // 为Dep类设置一个静态属性,默认为null,工作时指向当前的Watcher
```

2.实现一个订阅者 Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。
主要有两步：把 Watcher 添加到 Dep 容器中，这里我们用到了 监听器的 get 函数；接收到通知，执行更新函数。
```js
function Watcher(vm, prop, cb) {
  this.vm = vm;
  this.prop = prop;
  this.cb = cb;
  this.value = this.get();
}

Watcher.prototype = {
  update: function () {
    const value = this.vm.$data[this.prop];
    const oldVal = this.value;
    if (value !== oldVal) {
      this.value = value;
      this.cb(value);
    }
  },
  get: function () {
    Dep.target = this; // 储存订阅器
    const value = this.vm.$data[this.prop]; // 因为属性被监听，这一步会执行监听器里的 get方法
    Dep.target = null;
    return value;
  }
}
```

3.实现一个解析器 Compile，用来解析指令初始化模板，以及添加添加订阅者，绑定更新函数。
```js
// 在解析 DOM 节点的过程中我们会频繁的操作 DOM，利用文档片段 DocumentFragment 解析 DOM 优化性能
function Compile(vm) {
  this.vm = vm;
  this.el = vm.$el;
  this.fragment = null;
  this.init();
}

Compile.prototype = {
  init: function () {
    this.fragment = this.nodeFragment(this.el);
  },
  nodeFragment: function (el) {
    const fragment = document.createDocumentFragment();
    let child = el.firstChild;
    //将子节点，全部移动文档片段里
    while (child) {
      fragment.appendChild(child);
      child = el.firstChild;
    }
    return fragment;
  }
}

// 然后对整个节点和指令进行处理编译，根据不同的节点去调用不同的渲染函数，绑定更新函数，编译完成之后，再把 DOM 片段添加到页面中。
Compile.prototype = {
  compileNode: function (fragment) {
    let childNodes = fragment.childNodes;
    [...childNodes].forEach(node => {
      let reg = /\{\{(.*)\}\}/;
      let text = node.textContent;
      if (this.isElementNode(node)) {
        this.compile(node); //渲染指令模板
      } else if (this.isTextNode(node) && reg.test(text)) {
        let prop = RegExp.$1;
        this.compileText(node, prop); //渲染{{}} 模板
      }

      //递归编译子节点
      if (node.childNodes && node.childNodes.length) {
        this.compileNode(node);
      }
    });
  },
  compile: function (node) {
    let nodeAttrs = node.attributes;
    [...nodeAttrs].forEach(attr => {
      let name = attr.name;
      if (this.isDirective(name)) {
        let value = attr.value;
        if (name === "v-model") {
          this.compileModel(node, value);
        }
        node.removeAttribute(name);
      }
    });
  }
  // ...
}
```

4.数据代理
我们尝试去修改数据，也完全没问题，但是有个问题就是我们修改数据时是通过 vm.$data.name 去修改数据，而不是想 Vue 中直接用 vm.name 就可以去修改，那这个是怎么做到的呢？其实很简单，Vue 做了一步数据代理操作。
我们来改造下 Mvue 添加数据代理功能，我们也是利用 Object.defineProperty 方法进行一步中间的转换操作，间接的去访问。
```js
function Mvue(options) {
  this.$options = options;
  this.$data = options.data;
  this.$el = document.querySelector(options.el);
  //数据代理
  Object.keys(this.$data).forEach(key => {
    this.proxyData(key);
  });

  this.init();
}

Mvue.prototype = {
  init: function () {
    observer(this.$data);
    new Compile(this);
  },
  proxyData: function (key) {
    Object.defineProperty(this, key, {
      get: function () {
        return this.$data[key]
      },
      set: function (value) {
        this.$data[key] = value;
      }
    });
  }
}
```

5.实现方法二
实现一个简单vue的双向绑定功能，包括v-bind, v-model, v-click三个指令
```html
<!DOCTYPE html>
<head>
  <title>myVue</title>
</head>
<style>
  #app {
    text-align: center;
  }
</style>
<body>
  <div id="app">
    <form>
      <input type="text"  v-model="number">
      <button type="button" v-click="increment">增加</button>
    </form>
    <h3 v-bind="number"></h3>
  </div>
</body>

<script>
  function myVue(options) {
    this._init(options);
  }

  myVue.prototype._init = function (options) {
    this.$options = options;
    this.$el = document.querySelector(options.el);
    this.$data = options.data;
    this.$methods = options.methods;

    this._binding = {};
    this._obverse(this.$data);
    this._complie(this.$el);
  }
 
  myVue.prototype._obverse = function (obj) {
    var value;
    for (key in obj) {
      if (obj.hasOwnProperty(key)) {
        this._binding[key] = {
          _directives: []
        };
        value = obj[key];
        if (typeof value === 'object') {
          this._obverse(value);
        }
        var binding = this._binding[key];
        Object.defineProperty(this.$data, key, {
          enumerable: true,
          configurable: true,
          get: function () {
            console.log(`获取${value}`);
            return value;
          },
          set: function (newVal) {
            console.log(`更新${newVal}`);
            if (value !== newVal) {
              value = newVal;
              binding._directives.forEach(function (item) {
                item.update();
              })
            }
          }
        })
      }
    }
  }

  myVue.prototype._complie = function (root) {
    var _this = this;
    var nodes = root.children;
    for (var i = 0; i < nodes.length; i++) {
      var node = nodes[i];
      if (node.children.length) {
        this._complie(node);
      }

      if (node.hasAttribute('v-click')) {
        node.onclick = (function () {
          var attrVal = nodes[i].getAttribute('v-click');
          return _this.$methods[attrVal].bind(_this.$data);
        })();
      }

      if (node.hasAttribute('v-model') && (node.tagName == 'INPUT' || node.tagName == 'TEXTAREA')) {
        node.addEventListener('input', (function(key) {
          var attrVal = node.getAttribute('v-model');
          _this._binding[attrVal]._directives.push(new Watcher(
            'input',
            node,
            _this,
            attrVal,
            'value'
          ))

          return function() {
            _this.$data[attrVal] =  nodes[key].value;
          }
        })(i));
      } 

      if (node.hasAttribute('v-bind')) {
        var attrVal = node.getAttribute('v-bind');
        _this._binding[attrVal]._directives.push(new Watcher(
          'text',
          node,
          _this,
          attrVal,
          'innerHTML'
        ))
      }
    }
  }

  function Watcher(name, el, vm, exp, attr) {
    this.name = name;         //指令名称，例如文本节点，该值设为"text"
    this.el = el;             //指令对应的DOM元素
    this.vm = vm;             //指令所属myVue实例
    this.exp = exp;           //指令对应的值，本例如"number"
    this.attr = attr;         //绑定的属性值，本例为"innerHTML"

    this.update();
  }

  Watcher.prototype.update = function () {
    this.el[this.attr] = this.vm.$data[this.exp];
  }

  window.onload = function() {
    var app = new myVue({
      el:'#app',
      data: {
        number: 0
      },
      methods: {
        increment: function() {
          this.number ++;
        },
      }
    })
  }
</script>
```

6. 双向绑定的方法有哪些
基于数据劫持的双向绑定。Object.defineProperty；ES6的Proxy 代理；Object.observe 已废弃

Angular基于脏检查的双向绑定。对常用的dom事件、xhr事件进行了封装，触发时会调用$digest cycle。在$digest流程中，Angular将遍历每个数据变量的watcher，比较它的新旧值。当新旧值不同时，触发listener函数，执行相关的操作。

KnockoutJS基于观察者模式的双向绑定

Ember基于数据模型的双向绑定

7. Proxy与Object.defineProperty的优劣对比
Proxy的优势:
可以直接监听对象而非属性
直接监听数组的变化
有多达13种拦截方法,不限于apply、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的
Proxy返回的是一个新对象,我们可以只操作新的对象达到目的,而Object.defineProperty只能遍历对象属性直接修改
Proxy作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利

Object.defineProperty的优势: 兼容性好,支持IE9

8. Vue、Angular 和 React的区别
1.与AngularJS的区别
相同点：都支持指令：内置指令和自定义指令；都支持过滤器：内置过滤器和自定义过滤器；都支持双向数据绑定；都不支持低端浏览器。

不同点：AngularJS的学习成本高，比如增加了Dependency Injection特性，而Vue本身提供的API都比较简单、直观；在性能上，AngularJS依赖对数据做脏检查，所以Watcher越多越慢；Vue 使用基于依赖追踪的观察并且使用异步队列更新，所有的数据都是独立触发的。

2.与React的区别
相同点：React采用特殊的JSX语法，Vue 在组件开发中也推崇编写.vue特殊文件格式，对文件内容都有一些约定，两者都需要编译后使用；中心思想相同：一切都是组件，组件实例之间可以嵌套；都提供合理的钩子函数，可以让开发者定制化地去处理需求；都不内置列数AJAX，Route等功能到核心包，而是以插件的方式加载；在组件开发中都支持mixins的特性。

不同点：React采用的Virtual DOM会对渲染出来的结果做脏检查；Vue 在模板中提供了指令，过滤器等，可以非常方便，快捷地操作Virtual DOM。


9. 对keep-alive 的了解
keep-alive是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。在vue 2.1.0 版本之后，keep-alive新加入了两个属性: include(包含的组件缓存) 与 exclude(排除的组件不缓存，优先级大于include) 

10. $route和$router的区别
$route是路由信息对象，包括path，params，hash，query，fullPath，matched，name等路由信息参数。$router是路由实例对象，包括了路由的跳转方法，钩子函数等。

11. vue中 key 值的作用
跟踪每个节点的身份，从而重用和重新排序现有元素

12. 虚拟DOM实现原理?
虚拟DOM本质上是 JS 对象,是对真实DOM的抽象
状态变更时，记录新树和旧树的差异
最后把差异更新到真正的dom中

13. 虚拟DOM的优劣如何
优点:
保证性能下限: 虚拟DOM可以经过diff找出最小差异,然后批量进行patch,这种操作虽然比不上手动优化,但是比起粗暴的DOM操作性能要好很多,因此虚拟DOM可以保证性能下限
无需手动操作DOM: 虚拟DOM的diff和patch都是在一次更新中自动进行的,我们无需手动操作DOM,极大提高开发效率
跨平台: 虚拟DOM本质上是JavaScript对象,而DOM与平台强相关,相比之下虚拟DOM可以进行更方便地跨平台操作,例如服务器渲染、移动端开发等等

缺点: 无法进行极致优化: 在一些性能要求极高的应用中虚拟DOM无法进行针对性的极致优化,比如VScode采用直接手动操作DOM的方式进行极端的性能优化