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

3. 怎么实现对对象的拷贝(浅拷贝与深拷贝)
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

4. 什么是闭包，为什么要用它
简单来说，闭包就是能够读取其他函数内部变量的函数
由于 JavaScript 特殊的作用域，函数外部无法直接读取内部的变量，内部可以直接读取外部的变量，从而就产生了闭包的概念
最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中
由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露

5. 介绍一下 JavaScript 原型，原型链
每一个构造函数都拥有一个prototype属性，这个属性指向一个对象，也就是原型对象prototype；原型对象默认拥有一个constructor属性，指向它的那个构造函数；每个对象(实例)都拥有一个隐藏的属性__prototype__，指向它的原型对象。Person.prototype构造函数的原型属性与p.__proto__ 实例对象的原型对象是一个东西，只是从不同的角度访问原型。

JavaScript中所有的对象都是由它的原型对象继承而来。而原型对象自身也是一个对象，它也有自己的原型对象，这样层层上溯，就形成了一个类似链表的结构，这就是原型链
所有原型链的终点都是Object函数的prototype属性。Objec.prototype指向的原型对象同样拥有原型，不过它的原型是null，而null则没有原型。

6. JavaScript 如何实现继承
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

7. new 操作符具体干了什么
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

8. Object.create() 如何实现的
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

9. ({}+{}).length
* 2018最新Web前端经典面试试题及答案-史上最全前端面试题(含答案)， https://blog.csdn.net/xm1037782843/article/details/80708533
* 2018最新Web前端经典面试试题及答案， https://blog.csdn.net/wdlhao/article/details/79079660
```JavaScript
解析：
1、数+数 = 数（int float）
2、数+null = 数
3、数+其他数据类型 = string （强制转换成string 再相加）
4、其他数据类型 + 其他数据类型 = string(强制转换成string 再相加)
答案：({}+{}).length 等价于 ({}.toString() + {}.toString()).length，{}.toString()的值为[object Object]，所以最后结果为30。
```


