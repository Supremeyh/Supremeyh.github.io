---
title: 面试题 JavaScript
comments: true
date: 2019-03-19 22:19:20
categories: Review
tags: ['Review', 'JS']
---

1. JavaScript 有哪些数据类型
6种原始数据类型：Boolean、Number、String、Null、Undefined、Symbol: 符号(Symbols)是ECMAScript第6版新定义的。符号类型是唯一的并且是不可修改的
引用类型：Object

2. 怎么判断不同的JS数据类型
* typeof操作符：返回一个字符串，表示未经计算的操作数的类型。
对于基本数据类型（除null外，被认为是一个空的对象引用），返回其本身的数据类型，函数对象返回 function ，其他对象均返回 Object。

* instanceof  用来判断A 是否是 B的实例，表达式为 A instanceof B，返回一个Boolean类型的值
测试构造函数的prototype属性是否出现在对象的原型链中的任何位置
instanceof 检测的是原型,只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型

* Object.prototype.toString  返回当前对象对应的字符串形式。默认情况下返回类型字符串
如，Object.prototype.toString.call(true) ; // [object Boolean]


3. 检测一个变量是否为 String 类型
```JavaScript
// typeof 并不能判断出所有的 String 变量，无法检测用 new String(str)生成的字符串
// new String(str)：当 String() 和运算符 new 一起作为构造函数使用时，它返回一个新创建的 String 对象，存放的是字符串 str 或 str 的字符串表示。
// String(str)：当不用 new 运算符调用 String() 时，它只把 str 转换成原始的字符串，并返回转换后的值。

function isString(str) {
  // null 和 undefined 没有 constructor 属性
  if(str === null || str === undefined) { return false }
  return (str.constructor === String)
}
```
4. 如何判断Javascript对象是否存在
```JavaScript
// 适用于两种情况: x 从来没有出现过；x 只是用var声明了，但没有赋值
if (typeof(x) == 'undefined') {
  console.log('对象不存在')
}

// JavaScript 遇到预期为布尔值的地方（比如if语句的条件部分），就会将非布尔值的参数自动转换为布尔值。系统内部会自动调用Boolean函数。
// 如果 x 不存在（未声明）则会出错。 "", 0, NaN, null, undefined, false 作为if的条件的时候，被认为是false
if (x) { //... }
```

5. 怎么实现对对象的拷贝(浅拷贝与深拷贝)
* 浅拷贝。 拷贝原对象的引用，复制引用（指针），而未复制真正的值，改变原对象以后，新对象跟着改变。
var obj1 = obj2   赋值运算符= 
Array.prototype.slice() 
Array.prototype.concat() 
Object.assign()  只拷贝对象自身的可枚举的属性

* 深拷贝
JSON.parse(JSON.stringify(obj))。 复合类型的值只能是数组或对象，不能是函数、正则、日期对象，也不能是undefined
展开运算符...   取出参数对象的所有可遍历属性，拷贝到当前对象之中
immutable  改变新的对象不会改变原对象，在大量深拷贝操作中显著地减少性能消耗
递归来实现每一层都重新创建对象并赋值，真正意义上的深拷贝
```JavaScript
function deepClone(source){
  const targetObj = source.constructor === Array ? [] : {}; // 判断复制的目标是数组还是对象
  for(let keys in source){ // 遍历目标
    if(source.hasOwnProperty(keys)){
      if(source[keys] && typeof source[keys] === 'object'){ // 如果值是对象，就递归一下
        targetObj[keys] = source[keys].constructor === Array ? [] : {};
        targetObj[keys] = deepClone(source[keys]);
      }else{ // 如果不是，就直接赋值
        targetObj[keys] = source[keys];
      }
    }
  }
  return targetObj;
}
```

6. 什么是闭包，为什么要用它
简单来说，闭包就是能够读取其他函数内部变量的函数
由于 JavaScript 特殊的作用域，函数外部无法直接读取内部的变量，内部可以直接读取外部的变量，从而就产生了闭包的概念
最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中
由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露

7. 介绍一下 JavaScript 原型，原型链
每一个构造函数都拥有一个prototype属性，这个属性指向一个对象，也就是原型对象prototype；原型对象默认拥有一个constructor属性，指向它的那个构造函数；每个对象(实例)都拥有一个隐藏的属性__prototype__，指向它的原型对象。Person.prototype构造函数的原型属性与p.__proto__ 实例对象的原型对象是一个东西，只是从不同的角度访问原型。

JavaScript中所有的对象都是由它的原型对象继承而来。而原型对象自身也是一个对象，它也有自己的原型对象，这样层层上溯，就形成了一个类似链表的结构，这就是原型链
所有原型链的终点都是Object函数的prototype属性。Objec.prototype指向的原型对象同样拥有原型，不过它的原型是null，而null则没有原型。

8. JavaScript 如何实现继承
JavaScript语言不像面向对象的编程语言中有类的概念，所以也就没有类之间直接的继承，JavaScript中只有对象，使用函数模拟类，基于对象之间的原型链来实现继承关系，
ES6的语法中新增了class关键字，但也只是语法糖，内部还是通过函数和原型链来对类和继承进行实现。
* 原型链继承
最简单的继承实现方式。来自原型对象的所有属性被所有实例共享；创建子类实例时，无法向父类构造函数传参；原型链继承多个实例的引用类型属性指向相同，存在篡改的可能；为子类新增属性和方法，必须要在new语句之后执行，不能放到构造器中。
```JavaScript
function Animal() {}
Animal.prototype.name = 'cat'
Animal.prototype.say = function() {console.log('hello')}
var cat = new Animal()
```

* 构造继承
使用call或apply方法，将父对象的构造函数绑定在子对象上.
```JavaScript
function Animal() {
    this.species = "动物"
}
function Cat(name, age) {
    Animal.call(this)
    this.name = name 
    this.age = age
}

var cat = new Cat('汤姆', 2)
```
* 组合继承
```JavaScript
function Animal() {
  this.species = "动物"
}

function Cat(name){
  Animal.call(this)
  this.name = name
}

Cat.prototype = new Animal() // 重写原型
Cat.prototype.constructor = Cat
// 如果没有Cat.prototype = new Animal()这一行，Cat.prototype.constructor是指向Cat的；
// 加了这一行以后，Cat.prototype.constructor指向Animal.这显然会导致继承链的紊乱，因此必须手动纠正，将Cat.prototype对象的constructor值改为Cat
```
* extends 继承
ES6新增继承方式，Class 可以通过extends关键字实现继承
```JavaScript
class Animal {
    
}

class Cat extends Animal {
    constructor() {
      super();   // 相当于 Animal.prototype.constructor.call(this)
  }
}
// 使用 extends 实现继承，必须添加 super 关键字定义子类的 constructor
```

9. new 操作符具体干了什么
```JavaScript
var Foo = function () { }
var foo = new Foo()  

// 创建了一个新的空对象
var obj = new Object() 
 // 设置原型链。将改空对象obj的 __proto__指向构造函数的原型
obj.__proto__ = Foo.prototype   // 或者 Object.setPrototypeOf(obj, Foo.prototype)
// 以该对象为上下文执行构造函数。 让Foo中的this指向新创建的obj对象，并执行Foo的函数体，属性和方法被加入到 this 引用的对象中
var result = Foo.call(obj)  
// 判断Foo的返回值类型，返回。如果有 return 出来东西是引用类型(对象)的话就直接返回 return 的内容，没有的话就返回创建的这个对象
return typeof result === 'object' ? result : obj  

// 用代码表示
function NewFunc(func){
  var ret = {};
  if (func.prototype !== null) {
    ret.__proto__ = func.prototype
  }
  var ret1 = func.apply(ret, Array.prototype.slice.call(arguments, 1))
  if ((typeof ret1 === "object" || typeof ret1 === "function") && ret1 !== null){
    return ret1
  }
  return ret
}
```

10. Object.create() 如何实现的
```JavaScript
// object.create(proto, properties) 使用指定的原型对象及额外的属性去创建一个新的对象
Object.create = function (obj, properties)  {
  var F = function () {}
  F.prototype = obj
  if (properties) {
     Object.defineProperties(F, properties)
  }
  return new F()
}

Object.create({}, {a: {value: 1}})  // {a: 1}
// Object.cerate()必须接收一个对象参数；可以通过Object.create(null) 创建一个干净的对象，也就是没有原型
```

11. ({}+{}).length
```JavaScript
解析：
1、数+数 = 数（int float）
2、数+null = 数
3、数+其他数据类型 = string （强制转换成string 再相加）
4、其他数据类型 + 其他数据类型 = string(强制转换成string 再相加)
答案：({}+{}).length 等价于 ({}.toString() + {}.toString()).length，{}.toString()的值为[object Object]，所以最后结果为30。
```

12. 内存泄露
当创建对象和字符串等时，JavaScript就会分配内存，并在不再使用时自动释放内存，这种机制被称为垃圾收集GC。回收机制有标记清除和引用计数。

内存泄露是指一块被分配的内存既不能使用，又不能回收，直到浏览器进程结束。一般是堆区内存泄漏，栈区不会泄漏。

四种常见的JavaScript内存泄漏
```JavaScript
// 1.意外的全局变量。当引用未声明的变量时，会在全局对象中创建一个新变量。在浏览器中，全局对象将是window
function leak(){
  'use strict';  // 解决方法一，开始处添加'use strict'启用严格模式
  a = 233; // 报错
  this.b  =  'bbb'; // 默认绑定this指向全局, 严格模式下this指向undefined
}
// 解决方法二，也可以手动释放全局变量的内存
window.a = undefined
delete window.b 


// 2.被遗忘的定时器或回调。当不需要setInterval或者setTimeout时，定时器没有被clear，定时器的回调函数以及内部依赖的变量都不能被回收，造成内存泄漏。
var someResource = getData()
var timer = setInterval(function() {
  var node = document.getElementById('Node')
  if(node) {
      node.innerHTML = JSON.stringify(someResource))
  }
  // node、someResource 存储了大量数据 无法回收
}, 1000)
clearInterval(timer)  // 在定时器完成工作的时候，手动清除定时器。


// 3. 闭包。闭包可以维持函数内局部变量，使其得不到释放。
// 将事件处理函数定义在外部，解除闭包,或者在定义事件处理函数的外部函数中，删除对dom的引用
function bindEvent() { 
  var ele = document.createElement('app')
  ele.onclick=function(){ 
    // ...
    // 这个函数中 可以访问外部的变量ele 所以它引用了ele,而ele又引用了它，因此这个事件绑定将会造成内存泄露
  } 
  ele = null  // 解决方法一
}

// 解决方法二，把onclick的函数写在bindEvent外
function bindEvent() { 
  var ele = document.createElement("app"); 
  ele.onclick = onclickHandler
} 
function onclickHandler(){ 
  // ...
}


// 4.循环引用。函数将间接引用所有它能访问的对象。一个DOM对象被一个JS对象引用，同时又引用同一个或其它的JS对象。
// 要想破坏循环引用，引用DOM元素的对象或DOM对象的引用需要被赋值为null。


// 5.没有清理DOM元素引用
// 有时，保存 DOM 节点内部数据结构很有用。假如你想快速更新表格的几行内容，把每一行 DOM 存成字典（JSON 键值对）或者数组很有意义。此时，同样的 DOM 元素存在两个引用：一个在 DOM 树中，另一个在字典中。将来你决定删除这些行时，需要把两个引用都清除。
var elements = {
  button: document.getElementById('button'),
  image: document.getElementById('image'),
  text: document.getElementById('text')
};
function doStuff() {
  image.src = 'http://some.url/image';
  button.click();
  console.log(text.innerHTML);
  // ...
}
function removeButton() {
  document.body.removeChild(document.getElementById('button'));
  // 此时，仍旧存在一个全局的 #button 的引用elements 字典。button 元素仍旧在内存中，不能被 GC 回收。
}


// 6. 子元素存在引用引起的内存泄漏
// 此外还要考虑 DOM 树内部或子节点的引用问题。假如你的 JavaScript 代码中保存了表格某一个 <td> 的引用。将来决定删除整个表格的时候，直觉认为 GC 会回收除了已保存的 <td> 以外的其它节点。实际情况并非如此：此 <td> 是表格的子节点，子元素与父元素是引用关系。由于代码保留了 <td> 的引用，导致整个表格仍待在内存中。保存 DOM 元素引用的时候，要小心谨慎。


// 7.console 控制台日志。过多的console，比如定时器的console会导致浏览器卡死。
// 解决：合理利用console，线上项目尽量少的使用console
console.log('233');
```
13. 判断回文字符串
```JavaScript
// reverse
function Palindromes(str) {
  let reg = /[\W_]/g // \w 匹配所有字母和数字以及下划线； \W与之相反； [\W_] 表示匹配下划线或者所有非字母非数字中的任意一个；/g全局匹配
  let newStr = str.replace(reg, '').toLowerCase()
  let reverseStr = newStr.split('').reverse().join('')
  return reverseStr === newStr // 与 newStr 对比
}

// for 循环
// 写法一
function Palindromes(str) {
  let reg = /[\W_]/g
  let newStr = str.replace(reg, '').toLowerCase()
  for(let i = 0, len = Math.floor(newStr.length / 2); i < len; i++) {
    if(newStr[i] !== newStr[newStr.length - 1 - i]) return false
  }
  return true
}
// 写法二
function Palindromes(str) {
  let reg = /[\W_]/g
  let newStr = str.replace(reg, '').toLowerCase();
  let len = newStr.length
  for(let i = 0, j = len - 1; i < j; i++, j--) {
    if(newStr[i] !== newStr[j]) return false
  }
  return true
}

// 递归
function palin(str) {
  let reg = /[\W_]/g
  let newStr = str.replace(reg, '').toLowerCase()
  let len = newStr.length
  while(len >= 1) {
    if(newStr[0] != newStr[len - 1]) {
      // len = 0 // 为了终止while循环 否则会陷入死循环
      return false
    } else {
      return palin(newStr.slice(1, len - 1)); 
      // 截掉收尾字符 再次比较收尾字符是否相等，直到字符串剩下一个字符（奇数项）或者 0 个字符（偶数项）
    }
  }
  return true
}
```

14. 求 a[b] 的值。对象键名称只能是字符串。
```JavaScript
var a={}, b={key:'b'}, c={key:'c'} 
a[b]=123
a[c]=456
console.log(a[b])  // 456
// 因为键名称只能是字符串，b、c单做键会调用toString()方法，得到的都是[object Object]。a[b]和a[c]都等价于a["[object Object]"]。

// 如果采用a.b和a.c的方式赋值，会把b和c这两个字符作为字段进行处理
a.b = 123
a.c = 456
console.log(a) // {b: 123, c: 456}
```

15. this 指向
```JavaScript
var hero = {
  _name: 'John Doe',
  getSecretIdentity: function (){
      return this._name
  }
}
var stoleSecretIdentity = hero.getSecretIdentity

console.log(stoleSecretIdentity())      // undefined
console.log(hero.getSecretIdentity())   // "John Doe"


// 将 getSecretIdentity 赋给 stoleSecretIdentity，等价于定义了 stoleSecretIdentity 函数：
var stoleSecretIdentity =  function (){
  return this._name
}
// stoleSecretIdentity 的上下文是全局环境，所以第一个输出 undefined。可通过 call 、apply 和 bind 等方式改变 stoleSecretIdentity 的this 指向。
// 第二个是调用对象的方法，输出 "John Doe"。
```

16. 给你一个 DOM 元素，创建一个能访问该元素所有子元素的函数，并且要将每个子元素传递给指定的回调函数。
```JavaScript
// 利用 深度优先搜索(Depth-First-Search) 实现
function Traverse(ele, cb) {
  cb(ele)
  var list = ele.children
  for (var i = 0; i < list.length; i++) {
      Traverse(list[i], cb) 
  }
}
```

17. 判断字符串中出现次数最多的字符及出现的次数
```JavaScript
// 方法一：利用json数据个数“键”唯一的特性
function getMostChart(str) {
  //定义一个json对象用于保存str的每一项以及出现次数。
  var json = {}
  //遍历str,循环其中的每一个字符，将某个字符的值及出现的个数拿出来作为json的key和value
  for(var i=0; i<str.length; i++){
    //判断json中是否有当前str的值
    if(!json[str.charAt(i)]) {
      //如果不存在 就将当前值添加到json中去，并设置1
      json[str.charAt(i)] = 1
    } else {
      //如果存在的话就让数组中已有的当前值的value值++
      json[str.charAt(i)] ++
    }
  }

  //存储出现次数最多的值和次数
  var number = '', num = 0
  //遍历json  使用打擂算法统计需要的值
  for(var k in json) {
    //如果当前项大于下一项
    if (json[k]>num) {
      //就让当前值更改为出现最多次数的值
      num = json[k]
      number = k
    }
  }

  return {
    number, num
  }
}


// 利用数组reduce()方法 同时应用一个函数针对数组的两个值(从左到右)
function getMostChart(str) {
  //定义一个json对象用于保存str的每一项以及出现次数
  var json = str.split('').reduce((acc, cur) => (acc[cur]++ || (acc[cur] = 1), acc), {})

  //存储出现次数最多的值和次数
  var number = '', num = 0
  //遍历json  使用打擂算法统计需要的值
  for(var k in json){
    //如果当前项大于下一项
    if (json[k]>num) {
      //就让当前值更改为出现最多次数的值
      num = json[k]
      number = k
    }
  }

  return {
    number, num
  }
}


// 利用正则表达式的replace对str的每一项进行检测
function getMostChart(str){
  var json = {}
  str.replace(/(\w{1})/g,function($1) {
    json[$1] ? json[$1]+=1 : json[$1] = 1
  })
  //存储出现次数最多的值和次数
  var number = '', num = 0
  //遍历json  使用打擂算法统计需要的值
  for(var k in json){
    //如果当前项大于下一项
    if (json[k]>num) {
      //就让当前值更改为出现最多次数的值
      num = json[k]
      number = k
    }
  }
  return {
    number, num
  }
}
```

18. 活动对象与变量对象什么区别
变量对象Variable object: JS执行上下文中都有个对象，函数内部标示符、形参、变量声明等都保存在一个叫做变量对象中，但它是引擎实现上的，不可在JS环境中访问到。
活动对象Activation Objec: 就是作用域链上正在被执行和引用的变量对象

19. this 指向
* 普通函数（非箭头函数）被调用时（即运行时）才会确定该函数内this的指向。
* 箭头函数中的this在函数定义的时候就已经确定，它this指向的是它的外层作用域this的指向。
* 要确定函数中this的指向，必须先找到该函数被调用的位置。
* 可以理解成 this 就是 call 一个函数时，传入的第一个参数，其他形式都是它的语法糖，可按照 转换代码 将其转换为call形式 func.call(context, p1, p2)
```JavaScript
// 第一种 fn() 普通全局函数 形式
var a = 1
function fn () {
  console.log(this.a)
}
fn()  // 1
// 非严格模式下，直接不带任何引用形式去调用函数，则this会指向全局对象。因为没有其他影响去改变this，this默认就是指向全局对象（浏览器window，Node中是global）
// 严格模式下，这个this其实是undefined的


// 第二种 obj.fn() 作为对象属性被调用 形式
var a = 1
function fn () {
  console.log(this.a)
}
var obj = {
  a: 2,
  fn
}
obj.fn()  // 2
// 这种形式对比起第一种，很明显fn()已经是名花有主的了！看清楚，是谁呼唤的fn()？没错，就是obj，所以this的指向就不言而喻了。一句话，谁去调用这个函数的，这个函数中的this就绑定到谁身上。

var a = 1
function fn () {
  console.log(this.a)
}
var obj = {
  a: 2,
  fn
}
var obj0 = {
  a: 3,
  obj 
}
obj0.obj.fn() // 2
// 即使是这种串串烧的形式，结果也是一样的，test()中的this只对直属上司（直接调用者obj）负责

var a = 1
function fn () {
    console.log(this.a)
}
var obj = {
    a: 2,
    fn
}
var testCopy = obj.fn
testCopy() // 1 
// 不管是不是在函数内部，只要是在全局环境下运行，this就是指顶层对象window。

var a = 1
function fn () {
  console.log(this.a)
}
var obj = {
  a: 2,
  fn
}
setTimeout(obj.fn) // 1
// window.setTimeout()和window.setInterval() 里面的this默认是window对象



// 第三种 o.call(obj)、o.apply(obj) 、o.bind(obj) 形式
var a = 1
function fn () {
  console.log(this.a)
}
var obj = {
  a: 2,
  fn
}
var testCopy = obj.fn
testCopy.call(obj)  // 2
// call方法可以改变this的指向，指定this指向对象obj，然后在对象obj的作用域中运行函数。
// call方法的参数，应该是一个对象。如果参数为空、null和undefined，则默认传入全局对象。如果call方法的参数是一个原始值，那么这个原始值会自动转成对应的包装对象，然后传入call方法


// 第四种 new fn() 构造函数 形式
var a = 1
function fn (a) {
  this.a = a
}
var b = new fn(2)
console.log(b.a) // 2
// 构造函数中的this，指的是被new出来的实例对象


// 第五种 箭头函数 形式
// 箭头函数的this，总是指向定义时所在的对象，而不是运行时所在的对象
var a = 1
var fn = () => {
  console.log(this.a)
}
var obj = {
  a: 2,
  test
}
obj.fn()  // 1 
// 箭头函数不会创建自己的this，箭头函数中的this在函数定义的时候就已经确定(继承自父执行上下文中的this)，而不是在执行函数的时候绑定。它this指向的是它的外层作用域this的指向。 箭头函数不能用call方法修改里面的this.
var x = 11
var obj= {
  x: 22,
  say: ()=>{
    console.log(this.x)
  }
}
obj.say() // 11
// 箭头函数本身所在的对象为obj，而obj的父执行上下文就是window，因此这里的this.x实际上表示的是window.x 

function foo() {
  setTimeout( () => {
    console.log("id:", this.id)
  }, 100)
}
var id = 21   // 箭头函数运行时所在的环境
foo.call({ id: 42 }) // 42   箭头函数定义时所在的环境
// 箭头函数位于foo函数内部。只有foo函数运行(被调用)后，它才会按照定义生成，所以foo运行时所在的对象，恰好是箭头函数定义时所在的对象。
```

20. undefined 和 not defined 的区别
声明但为赋值，为 undefined
未声明变量直接使用会抛出异常：x is not defined，如果没有处理异常，代码就停止运行了。





