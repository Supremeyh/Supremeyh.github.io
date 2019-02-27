---
title: JavaScript基础
comments: true
date: 2019-02-26 09:52:37
categories: JS
tags: ['JS', '基础']
---

### 概述
* 什么是 JavaScript 语言？
JavaScript (JS for short) is the programming language that enables web pages to respond to user interaction beyond the basic level. It was created in 1995 by Brendan Eich, and is today one of the most famous and used programming languages.

JavaScript 是一种轻量级的脚本语言。所谓“脚本语言”（script language），指的是它不具备开发操作系统的能力，而是只用来编写控制其他大型应用程序（比如浏览器）的“脚本”。

JavaScript 也是一种嵌入式（embedded）语言。它本身提供的核心语法不算很多，只能用来做一些数学和逻辑运算。JavaScript 本身不提供任何与 I/O（输入/输出）相关的 API，都要靠宿主环境（host）提供，所以 JavaScript 只合适嵌入更大型的应用程序环境，去调用宿主环境提供的底层 API。

> 编译型语言 vs 解释型语言
编译型语言，首先是将源代码编译compile生成机器指令，再由机器运行机器码 (二进制)。执行效率高。如C、C++
解释型语言，源代码先翻译成中间代码，再由解释器对中间代码进行解释运行。每次运行的时候都要逐行解释一遍，有良好的平台兼容性。如JavaScript、Python、PHP

> 动态类型语言 vs 静态类型语言
动态类型语言，是指数据类型的检查是在运行时做的。用动态类型语言编程时，不用给变量指定数据类型，该语言会在你第一次赋值给变量时，在内部记录数据类型。
静态类型语言，是指数据类型的检查是在运行前（如编译阶段）做的。

* JavaScript语言的历史
1995年5月，Brendan Eich只用了10天，就设计完成了这种语言的第一版。

1996年，样式表标准CSS第一版发布。
1997年，DHTML（Dynamic HTML，动态HTML）发布，允许动态改变网页内容。这标志着DOM模式（Document Object Model，文档对象模型）正式应用。

1997年7月，ECMAScript 1.0。
1998年6月，ECMAScript 2.0。格式修正，以使得其形式与ISO/IEC16262国际标准一致。
1999年12月，ECMAScript 3.0。强大的正则表达式，更好的文字链处理，新的控制指令，异常处理，错误定义更加明确，数输出的格式化及其它改变。

2001年，微软公司时隔5年之后，发布了IE浏览器的下一个版本Internet Explorer 6。
2002年，Mozilla项目发布了它的浏览器的第一版，后来起名为Firefox。
2003年，苹果公司发布了Safari浏览器的第一版。
2005年，Ajax方法（Asynchronous JavaScript and XML）正式诞生。Google Maps项目大量采用该方法。促成了Web 2.0时代的来临。
2005年，Apache基金会发布了CouchDB数据库。基于JSON格式的数据库，可以用JavaScript函数定义视图和索引，标识着NoSQL类型的数据库诞生。
2006年，jQuery函数库诞生，作者为John Resig。
2006年，微软公司发布IE 7，标志重新开始启动浏览器的开发。
2008年，V8编译器诞生。这是Google公司为Chrome浏览器而开发的，它的特点是让JavaScript的运行变得非常快。V8是开源的，拓展了JavaScript的应用领域。
2009年，Node.js项目诞生，创始人为Ryan Dahl，它标志着JavaScript可以用于服务器端编程，从此网站的前端和后端可以使用同一种语言开发。

2009年12月，ECMAScript 5.0。
2011年6月，ECMAscript 5.1，并且成为ISO国际标准（ISO/IEC 16262:2011）。到了2012年底，所有主要浏览器都支持ECMAScript 5.1版的全部功能。

2012年，SPA单页面应用程序框架开始崛起，AngularJS项目和Ember项目都发布了1.0版本。
2012年，微软发布TypeScript语言。该语言被设计成JavaScript的超集，这意味着所有JavaScipt程序，都可以不经修改地在TypeScript中运行。
2013年5月，Facebook发布UI框架库React，引入了新的JSX语法，使得UI层可以用组件开发。
2015年3月，Facebook公司发布了React Native项目，将React框架移植到了手机端，可以用来开发手机原生App。
2015年4月，Angular框架宣布，2.0版将基于微软公司的TypeScript语言开发，这等于为JavaScript语言引入了强类型。

2015年6月17日，ECMAScript 6，更名为 ECMAScript 2015。新增let、const、class、modules、arrow functions、rest argument、binary data、promises等等。这个标准从提出到批准，历时10年，而JavaScript语言从诞生至今也已经20年了。
2016年06月，ECMAScript 2016。完善ES6规范，两个新的功能：求幂运算符（*）和array.prototype.includes方法。
2017年06月：ECMAScript 2017。增加新的功能，如并发、原子操作、Object.values/Object.entries、字符串填充、promises、await/asyn等等。
2017年11月，所有主流浏览器全部支持 WebAssembly，这意味着任何语言都可以编译成 JavaScript，在浏览器运行。


### 基本语法
* 语句 和 表达式
JavaScript 程序的执行单位为行（line），也就是一行一行地执行。一般情况下，每一行就是一个语句。
语句（statement）是为了完成某种任务而进行的操作，以分号结尾，一个分号就表示一个语句结束。var a = 2 + 3;
表达式（expression），指一个为了得到返回值的计算式，不需要分号结尾，一旦在表达式后面添加分号，则引擎就将其视为语句。 2 + 3
* 变量
变量是对“值”的具名引用。变量就是为“值”起名，然后引用这个名字，就等同于引用这个值。变量的名字就是变量名。

注意，JavaScript 的变量名区分大小写，A和a是两个不同的变量。

* 变量提升
JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。
```JavaScript
// hoisting
console.log(a);
var a = 1;

// 真正运行的是下面的代码
var a;
console.log(a);  // undefined
a = 1;
```
* 标识符
标识符（identifier）指的是用来识别各种值的合法名称。最常见的标识符就是变量名，以及后面要提到的函数名。JavaScript 语言的标识符对大小写敏感，所以a和A是两个不同的标识符。

规则：第一个字符，可以是任意 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号（$）和下划线（_）。第二个字符及后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字0-9。

中文是合法的标识符，可以用作变量名。  var 临时变量 = 1;

JavaScript 有一些保留字，不能用作标识符：
arguments、break、case、catch、class、const、continue、debugger、default、delete、do、else、enum、eval、export、extends、false、finally、for、function、if、implements、import、in、instanceof、interface、let、new、null、package、private、protected、public、return、static、super、switch、this、throw、true、try、typeof、var、void、while、with、yield。

* 循环语句
while 循环，只要条件为真，就不断循环执行代码块
```JavaScript
while (true) {
  console.log('Hello, world');
}
```
for 循环, 可以指定循环的起点、终点和终止条件, for语句的三个部分（initialize、test、increment），可以省略任何一个，也可以全部省略
```JavaScript
var x = 3;
for (var i = 0; i < x; i++) {
  console.log(i);
}

// 所有for循环，都可以改写成while循环
var x = 3;
var i = 0;
while (i < x) {
  console.log(i);
  i++;
}
```
do...while 循环，与while循环类似，唯一的区别就是先运行一次循环体，然后判断循环条件。至少运行一次，while语句后面的分号注意不要省略
```JavaScript
var x = 3;
var i = 0;
do {
  console.log(i);
  i++;
} while(i < x);
```
break 语句和 continue 语句，都具有跳转作用，可以让代码不按既有的顺序执行
break语句用于跳出代码块或循环。for循环也可以使用break语句跳出循环。
continue语句用于立即终止本轮循环，返回循环结构的头部，开始下一轮循环。

标签 label，语句的前面有标签，相当于定位符，用于跳转到程序的任意位置。标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。通常与break语句和continue语句配合使用，跳出特定的循环。
```JavaScript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }

// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0

// 上面代码为一个双重循环区块，break命令后面加上了top标签（注意，top不用加引号），满足条件时，直接跳出双层循环。如果break语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。
```

### 数据类型
* 七种数据类型
共有七种: 数值 number、字符串 string、布尔值 boolean、undefined、uull、symbol 和 对象 object
基本类型: number、string、boolean、undefined、uull、symbol。 按值访问，可以操作保存在变量中的实际的值，任何方法都无法改变一个基本类型的值，变量存放在栈区。
引用类型, Object、Array和Function。 按引用访问，值是同时保存在栈内存和堆内存中的对象，比较是引用的比较即堆内存中的地址是否相同

* typeof 运算符
JavaScript 有三种方法，可以确定一个值到底是什么类型。typeof运算符、instanceof运算符、Object.prototype.toString方法。

```JavaScript
// 数值、字符串、布尔值分别返回number、string、boolean
typeof 123 // "number"
typeof '123' // "string"
typeof false // "boolean"

// undefined返回undefined
typeof undefined // "undefined"  利用这一点，typeof可以用来检查一个没有声明的变量，而不报错 if (typeof v === "undefined") {}

// 对象返回object
typeof window // "object"
typeof {} // "object"
typeof [] // "object"   这表示在 JavaScript 内部，数组本质上只是一种特殊的对象

// instanceof运算符可以区分数组和对象
var o = {}, a = [];
o instanceof Array // false
a instanceof Array // true

// null返回object
typeof null // "object"   由于历史原因造成的,第一版没有null


// 函数返回function。
function f() {}
typeof f // "function"
```

* null, undefined 和布尔值
null是一个表示“空”的对象，表示空值，即该处的值现在为空，转为数值时为0
undefined是一个表示"未定义"的原始值，转为数值时为NaN
```JavaScript
Number(undefined) // NaN
5 + undefined // NaN
```

布尔值代表“真”和“假”两个状态， true 和 false
false: undefined、null、false、0、NaN、""或''（空字符串）
true:  除上面六个值外。  注意，空数组（[]）和空对象（{}）对应的布尔值都是true
```JavaScript
if ([]) {
  console.log('ok')  // ok
}
```
以下运算符会返回布尔值: 前置逻辑运算符 (! Not)、 相等运算符（===，!==，==，!=）、 比较运算符（>，>=，<，<=）

* 数值
JavaScript内部，所有数字都是以64位浮点数形式储存，即使整数也是如此，就是说，JS语言的底层根本没有整数，所有数字都是小数（64位浮点数）。。
```JavaScript
1 === 1.0 // true  1与1.0是相同的，是同一个数。

// 由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心
0.1 + 0.2 === 0.30000000000000004
0.2-0.1 === 0.1
0.3-0.2 === 0.09999999999999998
```
JavaScript 浮点数的64个二进制位
第1位：符号位，0表示正数，1表示负数
第2位到第12位（共11位）：指数部分，大小范围就是0 - 2047(2的11次方减1)
第13位到第64位（共52位）：小数部分（即有效数字）

数值精度: 
最多只能到53个二进制位，这意味着，绝对值小于2的53次方的整数，即-253到253，都可以精确表示。大于2的53次方的数值，都无法保持精度。
由于2的53次方是一个16位的十进制数值，所以简单的法则就是，JavaScript 对15位的十进制数都可以精确处理。

数值范围:
JavaScript 能够表示的数值范围为(2^1024,2^-1023)，超出这个范围的数无法表示。
如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回Infinity
如果一个数小于等于2的-1075次方（指数部分最小值-1023，再加上小数部分的52位），那么就会发生为“负向溢出”，这时会直接返回0。

JavaScript 提供Number对象的MAX_VALUE和MIN_VALUE属性，返回可以表示的具体的最大值和最小值。
Number.MAX_VALUE // 1.7976931348623157e+308
Number.MIN_VALUE // 5e-324

正零和负零:
JavaScript 的64位浮点数之中，有一个二进制位是符号位。这意味着，任何一个数都有一个对应的负值，就连0也不例外。
JavaScript 内部实际上存在2个0：一个是+0，一个是-0，区别就是64位浮点数表示法的符号位不同。它们是等价的。
几乎所有场合，正零和负零都会被当作正常的0。唯一有区别的场合是，+0或-0当作分母，返回的值是不相等的。
```JavaScript
0 === +0  === -0  // true
+0  // 0
-0  // 0
(-0).toString() // '0'

// 唯一有区别的场合是，+0或-0当作分母，返回的值是不相等的。因为除以正零得到+Infinity，除以负零得到-Infinity，这两者是不相等的
(1 / +0) === (1 / -0) // false   
``` 

NaN:
NaN是 JavaScript 的特殊值，表示“非数字”（Not a Number）
```JavaScript
// 主要出现在将字符串解析成数字出错的场合
5 - 'x' // NaN

// 一些数学函数的运算结果会出现NaN
Math.acos(2) // NaN   

// 0除以0也会得到NaN。
0 / 0 // NaN

// 需要注意的是，NaN不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number，使用typeof运算符可以看得很清楚。
typeof NaN // 'number'

// NaN不等于任何值，包括它本身。
NaN === NaN // false

// 数组的indexOf方法内部使用的是严格相等运算符，所以该方法对NaN不成立。
[NaN].indexOf(NaN) // -1

// NaN在布尔运算时被当作false。
Boolean(NaN) // false

// NaN与任何数（包括它自己）的运算，得到的都是NaN。
NaN + 666 // NaN
}
```

Infinity:
Infinity表示“无穷”，用来表示两种场景。一种是一个正的数值太大，或一个负的数值太小，无法表示；另一种是非0数值除以0，得到Infinity。
```JavaScript
Math.pow(2, 1024)  // Infinity

0 / 0  // NaN
1 / 0  // Infinity

// Infinity有正负之分，Infinity表示正的无穷，-Infinity表示负的无穷。
Infinity === -Infinity  // false
1 / -0  // -Infinity
-1 / -0  // Infinity

// Infinity大于一切数值（除了NaN），-Infinity小于一切数值（除了NaN）。
Infinity > 1000  // true
-Infinity < -1000  // true

// Infinity与NaN比较，总是返回false。
Infinity > NaN // false

// Infinity的四则运算，符合无穷的数学计算规则。
5 * Infinity // Infinity
5 - Infinity // -Infinity
Infinity / 5 // Infinity
5 / Infinity // 0

// 0乘以Infinity，返回NaN；0除以Infinity，返回0；Infinity除以0，返回Infinity。
0 * Infinity // NaN
0 / Infinity // 0
Infinity / 0 // Infinity

// Infinity加上或乘以Infinity，返回的还是Infinity。
Infinity + Infinity // Infinity
Infinity * Infinity // Infinity

// Infinity减去或除以Infinity，得到NaN。
Infinity - Infinity // NaN
Infinity / Infinity // NaN

// Infinity与null计算时，null会转成0，等同于与0的计算。
null * Infinity // NaN
null / Infinity // 0
Infinity / null // Infinity

// Infinity与undefined计算，返回的都是NaN。
Infinity / undefined // NaN
```

* 与数值相关的全局方法 
parseInt()  用于将字符串转为整数, 返回值只有两种可能，要么是一个十进制整数，要么是NaN。
```JavaScript
// 基本用法
parseInt('123') // 123

parseInt('   81') // 81    如果字符串头部有空格，空格会被自动去除。
parseInt(1.23) // 1   如果parseInt的参数不是字符串，则会先转为字符串再转换。

parseInt('15px') // 15   字符串转为整数的时候，是一个个字符依次转换，如果遇到不能转为数字的字符，就不再进行下去，返回已经转好的部分。
parseInt('abc') // NaN   如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回NaN。

parseInt('0x10') // 16  如果字符串以0x或0X开头，parseInt会将其按照十六进制数解析。
parseInt('011') // 11  如果字符串以0开头，将其按照10进制解析。

// 对于那些会自动转为科学计数法的数字，parseInt会将科学计数法的表示方法视为字符串，因此导致一些奇怪的结果。
parseInt(0.0000008) // 8
parseInt('8e-7') // 8

// 进制转换
// parseInt方法还可以接受第二个参数（2到36之间），表示被解析的值的进制，返回该值对应的十进制数。默认第二个参数为10，即默认是十进制转十进制
parseInt('1000', 2) // 8     二进制、八进制的1000，分别等于十进制的8、和512
parseInt('1000', 8) // 512

// 如果第二个参数不是数值，会被自动转为一个整数。这个整数只有在2到36之间，才能得到有意义的结果，超出这个范围，则返回NaN。如果第二个参数是0、undefined和null，则直接忽略。
parseInt('10', 37) // NaN
parseInt('10', 0) // 10
parseInt('10', null) // 10
parseInt('10', undefined) // 10

// 如果字符串包含对于指定进制无意义的字符，则从最高位开始，只返回可以转换的数值。如果最高位无法转换，则直接返回NaN。
parseInt('1546', 2) // 1
parseInt('546', 2) // NaN
```

parseFloat()：  用于将一个字符串转为浮点数
```JavaScript
parseFloat('3.14') // 3.14

// 如果字符串符合科学计数法，则会进行相应的转换
parseFloat('314e-2') // 3.14

// 如果字符串包含不能转为浮点数的字符，则不再进行往后转换，返回已经转好的部分
parseFloat('3.14more non-digit characters') // 3.14

// parseFloat方法会自动过滤字符串前导的空格
parseFloat('\t\v\r12.34\n ') // 12.34

// 如果参数不是字符串，或者字符串的第一个字符不能转化为浮点数，则返回NaN
parseFloat([]) // NaN
parseFloat('FF2') // NaN
parseFloat('') // NaN    尤其值得注意，parseFloat会将空字符串转为NaN


// 这些特点使得parseFloat的转换结果不同于Number函数
parseFloat(true)  // NaN
Number(true) // 1

parseFloat(null) // NaN
Number(null) // 0

parseFloat('') // NaN
Number('') // 0

parseFloat('123.45#') // 123.45
Number('123.45#') // NaN
```

isNaN():   可以用来判断一个值是否为NaN
```JavaScript
isNaN(NaN) // true
isNaN(123) // false

// 只对数值有效，如果传入其他值，会被先转成数值
isNaN('Hello') // true
// 相当于
isNaN(Number('Hello')) // true

// 出于同样的原因，对于对象和数组，isNaN也返回true
isNaN({}) // true
// 等同于
isNaN(Number({})) // true

// 但是，对于空数组和只有一个数值成员的数组，isNaN返回false ，因为这些数组能被Number函数转成数值
isNaN([]) // false
isNaN([123]) // false
isNaN(['123']) // false

// 因此，使用isNaN之前，最好判断一下数据类型
function myIsNaN(value) {
  return typeof value === 'number' && isNaN(value)
}

// 判断NaN更可靠的方法是，利用NaN为唯一不等于自身的值的这个特点，进行判断
function myIsNaN(value) {
  return value !== value
}
```

isFinite():  返回一个布尔值，表示某个值是否为正常的数值
```JavaScript
// 除了Infinity、-Infinity、NaN和undefined这几个值会返回false，isFinite对于其他的数值都会返回true
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false

isFinite(null) // true
isFinite(-1) // true
```


