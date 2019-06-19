---
title: 面试题 JavaScript
comments: true
date: 2019-04-10 11:00:00
categories: Review
tags: ['Review', 'JS']
---

1. 怎么判断不同的JS数据类型
* typeof操作符：返回一个字符串，表示未经计算的操作数的类型。
对于基本数据类型（除null外，被认为是一个空的对象引用），返回其本身的数据类型，函数对象返回 function ，其他对象均返回 Object。

* instanceof  用来判断A 是否是 B的实例，表达式为 A instanceof B，返回一个Boolean类型的值
测试构造函数的__prototype__属性是否出现在对象的原型链中的任何位置
instanceof 检测的是原型,只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型
```JavaScript
instanceof (A,B) = {
  var L = A.__proto__
  var R = B.prototype
  if(L === R) {
    // A的内部属性 __proto__ 指向 B 的原型对象
    return true;
  }
  return false;
}
```

* Object.prototype.toString  返回当前对象对应的字符串形式。默认情况下返回类型字符串
如，Object.prototype.toString.call(true) ; // [object Boolean]


2. 检测一个变量是否为 String 类型
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
3. 如何判断Javascript对象是否存在
```JavaScript
// 适用于两种情况: x 从来没有出现过；x 只是用var声明了，但没有赋值
if (typeof(x) == 'undefined') {
  console.log('对象不存在')
}

// JavaScript 遇到预期为布尔值的地方（比如if语句的条件部分），就会将非布尔值的参数自动转换为布尔值。系统内部会自动调用Boolean函数。
// 如果 x 不存在（未声明）则会出错。 "", 0, NaN, null, undefined, false 作为if的条件的时候，被认为是false
if (x) { //... }
```

4. 怎么实现对对象的拷贝(浅拷贝与深拷贝)
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

5. JavaScript 如何实现继承
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

6. new 操作符具体干了什么
```JavaScript
var Foo = function () { }
var foo = new Foo()  

// 创建了一个新的空对象
var obj = new Object() 
 // 设置原型链。将该空对象obj的 __proto__指向构造函数的原型
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

7. Object.create() 如何实现的
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

8. 判断回文字符串
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

9. 求 a[b] 的值。对象键名称只能是字符串。
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

10. this 指向
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

11. 给你一个 DOM 元素，创建一个能访问该元素所有子元素的函数，并且要将每个子元素传递给指定的回调函数。
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

12. 判断字符串中出现次数最多的字符及出现的次数
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

13. this 指向
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
  fn
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


14. 写一个mul函数，使用方法如下
console.log(mul(2)(3)(4)); // 24 
console.log(mul(4)(3)(4)); // 48
```JavaScript
// 返回一个匿名函数，运行这个匿名函数又返回一个匿名函数，最里面的匿名函数可以访问 x,y,z 进而算出乘积返回即可
function mul (x) {
  return function (y) {
    return function (z) {
      return x * y * z
    }
  }
}
// 函数是一等公民；函数可以有属性，并且能连接到它的构造方法；函数可以像一个变量一样存在内存中；函数可以当做参数传给其他函数；函数可以返回其他函数
```

15. forEach、for in、for of、Object.keys 的区别
* forEach 数组的方法，遍历数组，按升序为数组中含有效值的每一项执行一次callback 函数，那些已删除或者未初始化的项将被跳过。
三个形参分别代表当前的值、当前的索引、数组本身。
总是返回undefined，不能使用continue、break退出循环，不能使用return返回到外层。并且不可链式调用，典型用例是在一个链的最后执行副作用。
```JavaScript
arr.forEach((currentValue, index, arr)=>{
  // ...
})
```
* for in  以随机顺序遍历对象及原型链上的可枚举属性(键名)
只遍历可枚举属性
会遍历自定义、原型链上的属性，若只遍历自身可用 hasOwnProperty() 判断
遍历对象返回的属性名和遍历数组返回的index索引值都是字符串类型
不推荐在数组中使用 for in 遍历
```JavaScript
for (let key in obj) {
  if(obj.hasOwnProperty(key)) {
    // console.log(obj[key])
  }
}

// 举例
Array.prototype.getLength = function() {
  return this.length
}
var arr = ['a', 'b', 'c']
arr.name = 'June'
Object.defineProperty(arr, 'age', {
  enumerable: true,
  value: 17,
  writable: true,
  configurable: true
})

for(var i in arr) {
  console.log(i)  // 0, 1, 2, name, age, getLength
}
```
* for of  遍历可迭代对象自身的可迭代属性，并为每个不同属性值(键值)执行语句
可用于包括 Array、Map、Set、String、arguments、DOM NodeList对象 类似数组的对象等 的 可迭代对象，不支持普通对象
不会遍历到原型链上的属性
可以由break, continue、throw 或return终止迭代器
可搭配实例方法 entries()，同时输出数组的内容和索引；
```JavaScript
for (let val of obj) {
  // console.log(val)
}

// 举例
var obj = {name: 'June', age: 17, city: 'guangzhou'};
for(let [key, value] of Object.entries(obj)) {
  console.log(key, ':', value)  // name:June, age:17, city:guangzhou
}


// Object.entries(obj)：如果参数的数据结构具有键和值，则返回一个二元数组，数组的每个元素为参数的[key,value]数组，但Symbol 属性会被忽略
Object.entries({ [Symbol()]: 123, name: 'June', age: 17})
// [['name','June'], ['age', 17]]
```

* Object.keys  
返回对象自身可枚举属性组成的数组
不会遍历对象原型链上的属性以及 Symbol 属性
对数组的遍历顺序和 for in 一致
```JavaScript
// 举例
function Person() {
  this.name = 'June'
}
Person.prototype.getName = function() {
  return this.name
}
var person = new Person();
Object.defineProperty(person, 'age', {
  enumerable: true,
  value: 17,
  writable: true,
  configurable: true
})

Object.keys(person)  // ['name', 'age']
```

16. 优先查找局部变量，再查找全局变量
```JavaScript
var name = 'World!';
(function () {
  // var name;  // 相当于
  console.log( name);
  if (typeof name === 'undefined') {
    var name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();
// 函数会首先搜索函数内部的变量，因为var变量名提升，所以它能找到name，但此时代码还没有运行到赋值那一步，所以name是undefined。
// 结果： Goodbye Jack


var name = 'World!';
(function () {
  console.log( name);
})();
// js局部变量和全局变量，优先查找局部变量。当函数内部未找到此变量声明或定义时就会再去向外部寻找。
// 结果：World!
```

17. map() 和 parseInt() 
```JavaScript
["1", "2", "3"].map(parseInt);

// 解析
arr.map(function callback(currentValue[, index[, array]]) {
  // ...
}[, thisArg])

// map接受两个参数, 一个回调函数 callback, 一个回调函数的this值。其中回调函数接受三个参数 currentValue, index, arrary

parseInt(string, radix)
// string 要被解析的字符串。 如果不是字符串会被转换，忽视空格符
// 基数 radix(可选). 表示要解析的数字的基数。介于 2 ~ 36 之间
// 如果省略 radix 参数或其值为 0，parseInt会根据string来判断数字的基数，默认以 10 为基础来解析
// 如果 string 以 “0x” 或 “0X” 开头，parseInt() 会把 string 的其余部分解析为十六进制的整数
// 如果 string 以 0 开头，parseInt() 将把它解析为十进制的整数
// 如果 string 以 1-9 的数字开头，parseInt() 将把它解析为十进制的整数
// 如果 radix 参数小于 2 或者大于 36，则 parseInt() 将返回 NaN
// 当string无法转成radix进制数时会返回NaN。

// map只传入了回调函数 parseInt, 回调函数的参数index索引值作了parseInt的基数radix，导致出现超范围的radix赋值和不合法的进制解析，因此返回NaN
// 可以用重新定义parseInt，来检验
var parseInt = function (string, radix) {
  return string + '-' + radix;
};
['1','2','3'].map(parseInt);
// 得到
["1-0", "2-1", "3-2"]
// 所以，本题即问
// parseInt('1', 0);  // radix=0,以10为基数解析
// parseInt('2', 1);  // radix=1<2,返回NaN
// parseInt('3', 2);  // 3无法转成二进制

// 结果: [1, NaN, NaN]
```

18. 函数的原型和它的prototype属性无关
```JavaScript
function f() {}
var a = f.prototype
var b = Object.getPrototypeOf(f)
a === b

// 用new创建f的实例的原型指向 f.prototype，即 f.prototype == Object.getPrototypeOf(new f())
// Object.getPrototypeOf(f)是 f 函数的原型，即  Function.prototype === Object.getPrototypeOf(f)
// 答案: false
```

19. 函数声明优先级高于变量声明
```JavaScript
function a(x) {
  return x * 2
}
var a
console.log(a)

// 函数声明优先级高于变量声明。js按从上到下执行，相当于先声明了一个函数a，再声明了一个变量a，但此变量没有赋值，因此不会覆盖，故打印出函数体a. 如果改为 var a = 1, 那么结果为1.
```

20. call() 参数为空、null或undefined，默认为全局变量。alert会调用toString()方法
```JavaScript
function a() {
  alert(this)
}
a.call(null)

// call() 参数为空、null或undefined，默认为全局变量。浏览器为window
// alert会调用toString()方法,结果为[object Window]。 若alert改为console,则为Window
```

21. delete 用删除对象属性
```JavaScript
(function(x){
    delete x
    return x
})(1)

// delete 用删除对象属性，不能删除函数中传递的参数
```

22. 闭包
```js
var name = "The Window";
　var object = {
　　 name : "My Object",
　　　getNameFunc: function(){
　　　　return function(){
　　　　　return this.name;
　　　　};
　　　},
    getName:function(){
      return this.name;
    }
　};
console.log(object.getNameFunc()());  // The Window
console.log(object.getName()());  // My Object
```