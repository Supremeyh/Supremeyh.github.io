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
#### 七种数据类型
共有七种: 数值 number、字符串 string、布尔值 boolean、undefined、uull、symbol 和 对象 object
基本类型: number、string、boolean、undefined、uull、symbol。 按值访问，可以操作保存在变量中的实际的值，任何方法都无法改变一个基本类型的值，变量存放在栈区。
引用类型, Object、Array和Function。 按引用访问，值是同时保存在栈内存和堆内存中的对象，比较是引用的比较即堆内存中的地址是否相同

####  typeof 运算符
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

####  null, undefined 和布尔值
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

####  数值
JavaScript内部，所有数字都是以64位浮点数形式储存，即使整数也是如此，就是说，JS语言的底层根本没有整数，所有数字都是小数（64位浮点数）。。
```JavaScript
1 === 1.0 // true  1与1.0是相同的，是同一个数。

// 由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心
0.1 + 0.2 === 0.30000000000000004
0.2-0.1 === 0.1
0.3-0.2 === 0.09999999999999998
```
* JavaScript 浮点数的64个二进制位
第1位：符号位，0表示正数，1表示负数
第2位到第12位（共11位）：指数部分，大小范围就是0 - 2047(2的11次方减1)
第13位到第64位（共52位）：小数部分（即有效数字）

* 数值精度: 
最多只能到53个二进制位，这意味着，绝对值小于2的53次方的整数，即-253到253，都可以精确表示。大于2的53次方的数值，都无法保持精度。
由于2的53次方是一个16位的十进制数值，所以简单的法则就是，JavaScript 对15位的十进制数都可以精确处理。

* 数值范围:
JavaScript 能够表示的数值范围为(2^1024,2^-1023)，超出这个范围的数无法表示。
如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回Infinity
如果一个数小于等于2的-1075次方（指数部分最小值-1023，再加上小数部分的52位），那么就会发生为“负向溢出”，这时会直接返回0。

* JavaScript 提供Number对象的MAX_VALUE和MIN_VALUE属性，返回可以表示的具体的最大值和最小值。
Number.MAX_VALUE // 1.7976931348623157e+308
Number.MIN_VALUE // 5e-324

* 正零和负零:
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

* NaN:
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

* Infinity:
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

####  字符串
字符串就是零个或多个排在一起的字符，放在单引号或双引号之中。

* 转义, 反斜杠（\）在字符串内有特殊含义，用来表示一些特殊字符，所以又称为转义符。
```JavaScript
\0 ：null（\u0000）
\b ：后退键（\u0008）
\f ：换页符（\u000C）
\n ：换行符（\u000A）
\r ：回车键（\u000D）
\t ：制表符（\u0009）
\v ：垂直制表符（\u000B）
\' ：单引号（\u0027）
\" ：双引号（\u0022）
\\ ：反斜杠（\u005C）

// 反斜杠还有三种特殊用法
\HHH     //  反斜杠后面紧跟三个八进制数（000到377），代表一个字符  HHH对应该字符的 Unicode 码点, 只能输出256种字符。 如\251表示版权符号
\xHH     //  \x后面紧跟两个十六进制数（00到FF），代表一个字符     HH对应该字符的 Unicode 码点，只能输出256种字符比如\xA9表示版权符号
\uXXXX   //  \u后面紧跟四个十六进制数（0000到FFFF），代表一个字符 XXXX对应该字符的 Unicode 码点，比如\u00A9表示版权符号

// 如果在非特殊字符前面使用反斜杠，则反斜杠会被省略。
'\a'  // "a"
``` 

* 字符集:
JavaScript 引擎内部，所有字符都用 Unicode 表示。不仅以 Unicode 储存字符，还允许直接在程序中使用 Unicode 码点表示字符。
每个字符在 JavaScript 内部都是以16位（即2个字节）的 UTF-16 格式储存。也就是说，JavaScript 的单位字符长度固定为16位长度，即2个字节。
对于码点在U+10000到U+10FFFF之间的字符，JavaScript 总是认为它们是两个字符（length属性为2）。所以处理的时候，必须把这一点考虑在内，也就是说，JavaScript 返回的字符串长度可能是不正确的。 如 '𝌆'.length === 2


* Base64 转码： 一种编码方法，可以将任意值转成 0～9、A～Z、a-z、+和/这64个字符组成的可打印字符。
文本里面包含一些不可打印的符号，比如 ASCII 码0到31的符号都无法打印出来，这时可以使用 Base64 编码，将它们转成可以打印的字符。
有时需要以文本格式传递二进制数据，那么也可以使用 Base64 编码。
使用它的主要目的，不是为了加密，而是为了不出现特殊字符，简化程序的处理。

JavaScript 原生提供两个 Base64 相关的方法， 但不适合非 ASCII 码的字符，会报错
btoa()：任意值转为 Base64 编码
atob()：Base64 编码转为原来的值
```JavaScript
var str = 'Hello World!';
btoa(str)  // "SGVsbG8gV29ybGQh"
atob('SGVsbG8gV29ybGQh') // "Hello World!
```
要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用encodeURIComponent、decodeURIComponent这两个方法
```JavaScript
// 任意值转为 Base64 编码
function b64Encode(str) {
  return btoa(encodeURIComponent(str));
}

// Base64 编码转为原来的值
function b64Decode(str) {
  return decodeURIComponent(atob(str));
}

b64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"
b64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"
```

#### 对象
对象就是一组“键值对”（key-value）成员的集合，是一种无序的复合数据集合。
对象的所有键名都是字符串（ES6 又引入了 Symbol 值也可以作为键名），所以加不加引号都可以。
如果键名是数值，会被自动转为字符串。如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），且也不是数字，则必须加上引号，否则会报错。

对象的每一个键名又称为“属性”（property），它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“方法”，它可以像函数那样调用。
属性可以动态创建，不必在对象声明时就指定。

##### 对象的引用
```JavaScript
// 如果不同的变量名指向同一个对象，那么它们都是这个对象的引用，也就是说指向同一个内存地址。修改其中一个变量，会影响到其他所有变量。
var o1 = {};
var o2 = o1;
o1.a = 1;
o2.a // 1

// 此时，如果取消某一个变量对于原对象的引用，不会影响到另一个变量
var o1 = {};
var o2 = o1;
o1 = 1;
o2 // {}

// 但是，这种引用只局限于对象，如果两个变量指向同一个原始类型的值。那么，变量这时都是值的拷贝
var x = 1;
var y = x;
x = 2;
y // 1
```
##### 属性的操作
* 读取对象的属性
有两种方法，一种是使用点运算符，还有一种是使用方括号运算符。
请注意，如果使用方括号运算符，键名必须放在引号里面，否则会被当作变量处理。方括号运算符内部还可以使用表达式。
数字键可以不加引号，因为会自动转成字符串，但数值键名不能使用点运算符（因为会被当成小数点），只能使用方括号运算符。
```JavaScript
var foo = 'bar';

var obj = {
  foo: 1,
  bar: 2
};

obj.foo   // 1  引用对象obj的foo属性时，如果使用点运算符，foo就是字符串
obj[foo]  // 2  如果使用方括号运算符，但是不使用引号，那么foo就是一个变量，指向字符串bar
```
* 属性的查看
Object.keys(obj): 返回对象自身的所有可枚举的属性名称的数组
Object.values(obj):  返回给定对象自身可枚举值的数组
Object.entries():  返回一个给定对象自身可枚举属性的键值对数组
```JavaScript
const obj = { foo: 'bar', baz: 42 }
Object.keys(obj)    //  ['foo', 'baz']
Object.values(obj)  //  ['bar', 42]
Object.entries(object1)  // [[ 'foo', 'bar' ], [ 'baz', 42 ]]

for (let key in obj) {
   console.log('key:', key, 'value: ',obj[key])  
   // key: foo  value: bar
   // key: baz  value: 42
}
```

* 属性的遍历
for...in  遍历对象自身的和继承的可枚举的属性。 对象obj继承了toString属性，该属性不会被for...in循环遍历到，因为它默认是“不可遍历”的。

* 属性是否存在：
in 运算符，如'name' in obj，检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回true，否则返回false。它不能识别哪些属性是对象自身的，哪些属性是继承的。

hasOwnProperty：是否为对象自身的属性。如 obj.hasOwnProperty('toString'))  false

* 属性的删除
delete: 删除对象本身的属性，无法删除继承的属性，删除成功后返回true。 注意，删除一个不存在的属性，delete不报错，而且返回true。只有一种情况，delete命令会返回false，那就是该属性存在，且不得删除。


#### 数组
数组（array）是按次序排列的一组值。每个值的位置都有编号（从0开始），整个数组用方括号表示。
本质上，数组属于一种特殊的对象。typeof运算符会返回数组的类型是object。  
JavaScript 使用一个32位整数，保存数组的元素个数。这意味着，数组成员最多只有 4294967295 个（2^32 - 1），length属性的最大值就是 4294967295。

清空数组的一个有效方法，就是将length属性设为0。

* in 运算符
检查某个键名是否存在的运算符in，适用于对象，也适用于数组。

* 数组的遍历
可以考虑使用for循环、while循环，或者forEach。 不推荐使用for...in遍历数组。
```JavaScript
var arr = [ 'a', 'b', 'c' ]
typeof arr // "object"


'2' in arr // true  数组存在键名为2的键
2 in arr  // true   由于键名都是字符串，所以数值2会自动转成字符串
4 in arr // false


var arr2 = [1, 2, 3];
arr2.foo = true;

// for循环
for(var i = 0; i < arr2.length; i++) {
  console.log(arr2[i]);
}

// while循环
var i = 0;
while (i < arr2.length) {
  console.log(arr2[i]);
  i++;
}

// 逆向遍历
var len = arr2.length;
while (len--) {
  console.log(arr2[len]);  // 3 2 1   
}

// forEach
arr2.forEach(function (val) {
  console.log(val)
})

// for...in循环不仅可以遍历对象，也可以遍历数组，毕竟数组只是一种特殊对象。但是，for...in不仅会遍历数组所有的数字键，还会遍历非数字键
for (var key in arr2) {
  console.log(key);   // 0 1 2 foo
}
```
* 数组的空位
当数组的某个位置是空元素，即两个逗号之间没有任何值，我们称该数组存在空位（hole）。
```JavaScript
var a = [1, , 2,]

// 数组的空位不影响length属性
a.length // 3   

// 使用delete命令删除一个数组成员，会形成空位，并且不会影响length属性
delete a[2]  // true
a[1] // undefined
a.length // 3
```
length属性不过滤空位。所以，使用length属性进行数组遍历，一定要非常小心
数组的某个位置是空位，与某个位置是undefined，是不一样的。
如果是空位，使用数组的forEach方法、for...in结构、以及Object.keys方法进行遍历，空位都会被跳过。如果某个位置是undefined，遍历的时候就不会被跳过,输出undefined。

* 类似数组的对象 array-like object
如果一个对象的所有键名都是正整数或零，并且有length属性，那么这个对象就很像数组，语法上称为“类似数组的对象”
但是，“类似数组的对象”并不是数组，因为它们不具备数组特有的方法。没有数组的push方法，使用该方法就会报错。length属性不是动态值，不会随着成员的变化而变。
典型的“类似数组的对象”是函数的arguments对象，以及大多数 DOM 元素集，还有字符串。
```JavaScript
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
}

// 数组的slice方法可以将“类似数组的对象”变成真正的数组。
var arr = Array.prototype.slice.call(arrayLike);

// 除了转为真正的数组，“类似数组的对象”还有一个办法可以使用数组的方法，就是通过call()把数组的方法放到对象上面。
var arr = Array.prototype.forEach.call(arrayLike, print)

function print(value, index) {
  console.log(index + ' : ' + value);
}
```

#### 函数
函数是一段可以反复调用的代码块。函数还能接受输入的参数，不同的参数会返回不同的值。

##### 函数的声明 
JavaScript 有三种声明函数的方法。
```JavaScript
// function 命令
function print(s) {
  console.log(s)
}

// 函数表达式   
// 采用函数表达式声明函数时，function命令后面不带有函数名。如果加上函数名，该函数名只在函数体内部有效，在函数体外部无效
var print = function(s) {
  console.log(s)
}; // 函数的表达式需要在语句的结尾加上分号，表示语句结束

// Function 构造函数   
// Function构造函数可以不使用new命令，返回结果完全一样
// 最后一个参数是函数的“函数体”，其他参数都是add函数的参数。如果只有一个参数，该参数就是函数体。
var add = new Function('x', 'y', 'return x + y');
// 等同于
function add(x, y) {
  return x + y;
}
```
函数的重复声明: 函数名视同变量名，函数名会提升。如果同一个函数被多次声明，后面的声明就会覆盖前面的声明。

第一等公民: 函数与其他数据类型地位平等，所以在 JavaScript 语言中又称函数为第一等公民。

* 函数的属性和方法
name: 函数的name属性返回函数的名字
```JavaScript
function f1() {}
f1.name // "f1"

// 如果是通过变量赋值定义的函数，那么name属性返回变量名
var f2 = function () {};
f2.name // "f2"

// 但是，上面这种情况，只有在变量的值是一个匿名函数时才是如此。如果变量的值是一个具名函数，那么name属性返回function关键字之后的那个函数名。
var f3 = function myName() {};
f3.name // 'myName'    返回了函数表达式的名字。注意，真正的函数名还是f3，而myName这个名字只在函数体内部可用。

// name属性的一个用处，就是获取参数函数的名字
var myFunc = function () {};
function test(f) {
  console.log(f.name);
}
test(myFunc) // myFunc  函数test内部通过name属性，就可以知道传入的参数是什么函数。
```
length： 函数的length属性返回函数预期传入的参数个数，即函数定义之中的参数个数。与实际传入的参数个数无关。

toString(): 函数的toString方法返回一个字符串，内容是函数的源码。函数内部的注释也可以返回。利用这一点，可以变相实现多行字符串。
```JavaScript
var multiline = function (fn) {
  var arr = fn.toString().split('\n');
  return arr.slice(1, arr.length - 1).join('\n');
};

function f() {/*
  这是一个
  多行注释
*/}

multiline(f);
// " 这是一个
//   多行注释"
```

* 函数本身的作用域
函数本身也是一个值，也有自己的作用域。它的作用域与变量一样，就是其声明时/定义时所在的作用域，与其运行时/调用时所在的作用域无关。
```JavaScript
var a = 1;
var x = function () {
  console.log(a);
};

function f() {
  var a = 2;
  x();
}

f() // 1   函数x是在函数f的外部声明的，所以它的作用域绑定外层，内部变量a不会到函数f体内取值，所以输出1，而不是2。
```

* 参数的传递方式
```JavaScript
// 参数如果是原始类型的值（数值、字符串、布尔值），传递方式是传值传递（passes by value）。这意味着，在函数体内修改参数值，不会影响到函数外部。
var p1 = 2;
function f(p1) {
  p1 = 3;
}
f(p1);
p1 // 2

// 参数是复合类型的值（数组、对象、其他函数），传递方式是传址传递（pass by reference）。传入函数的原始值的地址，在函数内部修改参数，将会影响到原始值。
var obj1 = { p2: 1 };
function f(o) {
  o.p2 = 2;
}
f(obj1);
obj1.p2 // 2

// 如果函数内部修改的，不是参数对象的某个属性，而是替换掉整个参数，这时不会影响到原始值
var obj2 = [1, 2, 3];
function f(o) {
  o = [2, 3, 4];
}
f(obj2);
obj2 // [1, 2, 3]  这是因为，形式参数（o）的值实际是参数obj的地址，重新对o赋值导致o指向另一个地址，保存在原地址上的值当然不受影响
```

* arguments 对象
由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是arguments对象的由来。
arguments对象包含了函数运行时的所有参数，arguments[0]就是第一个参数，arguments[1]就是第二个参数，以此类推。这个对象只有在函数体内部，才可以使用。
通过arguments对象的length属性，可以判断函数调用时到底带几个参数。
正常模式下，arguments对象可以在运行时修改。严格模式下，arguments对象与函数参数不具有联动关系。也就是说，修改arguments对象不会影响到实际的函数参数。

需要注意的是，虽然arguments很像数组，但它是一个对象。数组专有的方法（比如slice和forEach），不能在arguments对象上直接使用。如果要让arguments对象使用数组方法，真正的解决方法是将arguments转为真正的数组。
```JavaScript
// slice方法
var args = Array.prototype.slice.call(arguments);

// 逐一填入新数组
var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
```

* 闭包  closure
理解闭包，首先必须理解变量作用域。前面提到，JavaScript 有两种作用域：全局作用域和函数作用域。函数内部可以直接读取全局变量。但是，函数外部无法读取函数内部声明的变量。

如果出于种种原因，需要得到函数内的局部变量。正常情况下，这是办不到的，只有通过变通方法才能实现。那就是在函数的内部，再定义一个函数。

这就是 JavaScript 语言特有的"链式作用域"结构（chain scope），子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。

既然f2可以读取f1的局部变量，那么只要把f2作为返回值，我们不就可以在f1外部读取它的内部变量了吗！
```JavaScript
function f1() {
  var n = 999;
  // 闭包就是函数f2，即能够读取其他函数内部变量的函数。
  function f2() {  
    console.log(n);
  }
  return f2; 
}

var result = f1();
result(); // 999
```
由于在 JavaScript 语言中，只有函数内部的子函数才能读取内部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。闭包最大的特点，就是它可以“记住”诞生的环境，比如f2记住了它诞生的环境f1，所以从f2可以得到f1的内部变量。在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

闭包的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在。闭包可以看作是函数内部作用域的一个接口，闭包使得内部变量记住上一次调用时的运算结果。始终在内存中，不会在调用结束后，被垃圾回收机制回收。

闭包的另一个用处，是封装对象的私有属性和私有方法。
```JavaScript
function Person(name) {
  var _age;
  function setAge(n) {
    _age = n;
  }
  function getAge() {
    return _age;
  }

  return {
    name: name,
    getAge: getAge,
    setAge: setAge
  };
}

var p1 = Person('张三');
p1.setAge(25);
p1.getAge() // 25

// 上面代码中，函数Person的内部变量_age，通过闭包getAge和setAge，变成了返回对象p1的私有变量。
```
注意，外层函数每次运行，都会生成一个新的闭包，而这个闭包又会保留外层函数的内部变量，所以内存消耗很大。因此不能滥用闭包，否则会造成网页的性能问题。


* 立即调用的函数表达式 IIFE （Immediately-Invoked Function Expression）
在 JavaScript 中，圆括号()是一种运算符，跟在函数名之后，表示调用该函数。比如，print()就表示调用print函数。

有时，我们需要在定义函数之后，立即调用该函数。这时，你不能在函数的定义之后加上圆括号，这会产生语法错误。
```JavaScript
function(){ /* code */ }();  // Uncaught SyntaxError: Unexpected token (
// 产生这个错误的原因是，function这个关键字即可以当作语句，也可以当作表达式。
// 语句
function f() {}

// 表达式
var f = function f() {}
```
为了避免解析上的歧义，JavaScript 引擎规定，如果function关键字出现在行首，一律解释成语句。因此，JavaScript 引擎看到行首是function关键字之后，认为这一段都是函数的定义，不应该以圆括号结尾，所以就报错了。

解决方法就是不要让function出现在行首，让引擎将其理解成一个表达式。最简单的处理，就是将其放在一个圆括号里面。
```JavaScript
// 下面两种写法都是以圆括号开头，引擎就会认为后面跟的是一个表示式，而不是函数定义语句，所以就避免了错误。这就叫做“立即调用的函数表达式”，简称 IIFE。
(function(){ /* code */ }());

(function(){ /* code */ })();
// 注意，上面两种写法最后的分号都是必须的。如果省略分号，遇到连着两个 IIFE，可能就会报错。

// 推而广之，任何让解释器以表达式来处理函数定义的方法，都能产生同样的效果，比如下面三种写法。
!function () { /* code */ }();
~function () { /* code */ }();
-function () { /* code */ }();
+function () { /* code */ }();
```
通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的目的有两个：一是不必为函数命名，避免了污染全局变量；二是 IIFE 内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。


### 运算符
运算符是处理数据的基本方法，用来从现有的值得到新的值。JavaScript 提供了多种运算符，覆盖了所有主要的运算。
#### 算术运算符
JavaScript 共提供10个算术运算符，用来完成基本的算术运算。
加法x + y、 减法 x - y、 乘法 x * y、 除法x / y、 指数x ** y、 余数x % y、 自增++x 或者 x++、 自减--x 或者 x--、 数值 +x、 负数值-x

* 加法运算符
```JavaScript
// 加法运算符（+）是最常见的运算符，用来求两个数值的和。
1 + 1 // 2

// JavaScript 允许非数值的相加。
true + true // 2  布尔值都会自动转成数值，然后再相加
1 + true // 2

// 如果是两个字符串相加，这时加法运算符会变成连接运算符，返回一个新的字符串，将两个原字符串连接在一起。
'a' + 'bc' // "abc"

// 如果一个运算子是字符串，另一个运算子是非字符串，这时非字符串会转成字符串，再连接在一起。
1 + 'a' // "1a"
false + 'a' // "falsea"

// 加法运算符是在运行时决定，到底是执行相加，还是执行连接。也就是说，运算子的不同，导致了不同的语法行为，这种现象称为“重载”（overload）
'3' + 4 + 5 // "345"
3 + 4 + '5' // "75"

// 除了加法运算符，其他算术运算符（比如减法、除法和乘法）都不会发生重载。它们的规则是：所有运算子一律转为数值，再进行相应的数学运算。
1 - '2' // -1
1 * '2' // 2
1 / '2' // 0.5


// 对象的相加  如果运算子是对象，必须先转成原始类型的值，然后再相加。
var obj1 = { p: 1 };
obj1 + 2 // "[object Object]2"


// 对象转成原始类型的值，规则如下。
// 首先，自动调用对象的valueOf方法。一般来说，对象的valueOf方法总是返回对象自身，这时再自动调用对象的toString方法，将其转为字符串。对象的toString方法默认返回[object Object]
var obj1 = { p: 1 };
obj1.valueOf() // {p: 1}
obj1.valueOf().toString() // "[object Object]"

// 知道了这个规则以后，就可以自己定义valueOf方法或toString方法，得到想要的结果。
var obj2 = {
  valueOf: function () {
    return 1;
  }
};
obj2 + 2 // 3

var obj3 = {
  toString: function () {
    return 'hello';
  }
};
obj3 + 2 // "hello2"   只要有一个运算子是字符串，加法运算符就变成连接运算符，返回连接后的字符串。

// 这里有一个特例，如果运算子是一个Date对象的实例，那么会优先执行toString方法。
var obj4 = new Date();
obj4.valueOf = function () { return 1 };
obj4.toString = function () { return 'hello' };
obj4 + 2 // "hello2"
```
* 余数运算符
余数运算符（%）返回前一个运算子被后一个运算子除，所得的余数。
```JavaScript
12 % 5 // 2

// 需要注意的是，运算结果的正负号由第一个运算子的正负号决定。
-1 % 2 // -1
1 % -2 // 1

// 所以，为了得到负数的正确余数值，可以先使用绝对值函数。
function isOdd(n) {
  // return n % 2 === 1;   // 错误的写法
  return Math.abs(n % 2) === 1
}
```
* 自增和自减运算符
自增和自减运算符，是一元运算符，只需要一个运算子。它们的作用是将运算子首先转为数值，然后加上1或者减去1。它们会修改原始变量。

运算之后，变量的值发生变化，这种效应叫做运算的副作用（side effect）。自增和自减运算符是仅有的两个具有副作用的运算符，其他运算符都不会改变变量的值。

注意，运算符放在变量之后，会先返回变量操作前的值，再进行自增/自减操作；放在变量之前，会先进行自增/自减操作，再返回变量操作后的值。
```JavaScript
var x = 1;
var y = 1;

x++ // 1
++y // 2
```
#### 布尔运算符
布尔运算符用于将表达式转为布尔值，一共包含四个运算符。

取反运算符：!  
将布尔值变为相反值, 对于非布尔值，取反运算符会将其转为布尔值
```JavaScript
// undefined、null、false、0、NaN、空字符串（''）六个值取反后为true，其他值都为false。
!undefined // true
!null // true
!0 // true
!NaN // true
!"" // true

!54 // false
!'hello' // false
![] // false
!{} // false

// 对一个值连续做两次取反运算，等于将其转为对应的布尔值，与Boolean函数的作用相同。这是一种常用的类型转换的写法。
!!x
// 等同于
Boolean(x)
```
且运算符 &&   
它的运算规则是：如果第一个运算子的布尔值为true，则返回第二个运算子的值（注意是值，不是布尔值）；如果第一个运算子的布尔值为false，则直接返回第一个运算子的值，且不再对第二个运算子求值。
```JavaScript
if (i) {
  doSomething()
}
// 等价于
i && doSomething()

// 且运算符可以多个连用，这时返回第一个布尔值为false的表达式的值。如果所有表达式的布尔值都为true，则返回最后一个表达式的值。
true && 0 && 3  // 0
```
或运算符 ||
如果第一个运算子的布尔值为true，则返回第一个运算子的值，且不再对第二个运算子求值；如果第一个运算子的布尔值为false，则返回第二个运算子的值。
```JavaScript
// 或运算符可以多个连用，这时返回第一个布尔值为true的表达式的值。如果所有表达式都为false，则返回最后一个表达式的值。
false || 0 || '' || 4 || 'foo' || true     // 4

// 或运算符常用于为一个变量设置默认值。
function saveText(text) {
  text = text || '';
  // ...
}
// 或者写成
saveText(this.text || '')
```
三元运算符 ? :
JavaScript 语言唯一一个需要三个运算子的运算符。如果第一个表达式的布尔值为true，则返回第二个表达式的值，否则返回第三个表达式的值。

通常来说，三元条件表达式与if...else语句具有同样表达效果，前者可以表达的，后者也能表达。但是两者具有一个重大差别，if...else是语句，没有返回值；三元条件表达式是表达式，具有返回值。所以，在需要返回值的场合，只能使用三元条件表达式，而不能使用if..else
```JavaScript
't' ? 'hello' : 'world'  // "hello"
0 ? 'hello' : 'world'   // "world"

console.log(true ? 'T' : 'F')
// 上面代码中，console.log方法的参数必须是一个表达式，这时就只能使用三元条件表达式。如果要用if...else语句，就必须改变整个代码写法了。
```

#### 二进制位运算符
二进制位运算符用于直接对二进制位进行计算，一共有7个。

这些位运算符直接处理每一个比特位（bit），所以是非常底层的运算，好处是速度极快，缺点是很不直观，许多场合不能使用它们，否则会使代码难以理解和查错。

有一点需要特别注意，位运算符只对整数起作用，遇到小数时，会将小数部分舍去，只保留整数部分，如果一个运算子不是整数，会自动转为整数后再执行。另外，虽然在 JavaScript 内部，数值都是以64位浮点数的形式储存，但是做位运算的时候，是以32位带符号的整数进行运算的，并且返回值也是一个32位带符号的整数。

二进制或运算符（or）：符号为|，表示若两个二进制位之中只要有一个为1，则结果为1，否则为0
```JavaScript
0 | 3  // 3    0和3的二进制形式分别是00和11，所以进行二进制或运算会得到11（即3）
// 位运算只对整数有效，遇到小数时，会将小数部分舍去，只保留整数部分。所以，将一个小数与0进行二进制或运算，等同于对该数去除小数部分，即取整数位。
i = i | 0;  // 就是将i（不管是整数或小数）转为32位整数。  
2147483649.4 | 0;  // -2147483647   不适用超过32位整数最大值2147483647的数
```
二进制与运算符（and）：符号为&，表示若两个二进制位都为1，则结果为1，否则为0
```JavaScript
0 & 3 // 0    0（二进制00）和3（二进制11）进行二进制与运算会得到00（即0）
```
二进制否运算符（not）：符号为~，表示对一个二进制位取反（0变为1，1变为0）
```JavaScript
~ 3 // -4
// JavaScript 内部将所有的运算子都转为32位的二进制整数再进行运算。 3的32位整数形式是00000000000000000000000000000011，二进制否运算以后得到11111111111111111111111111111100。由于第一位（符号位）是1，所以这个数是一个负数。JavaScript 内部采用补码形式表示负数，即需要将这个数减去1，再取一次反，然后加上负号，才能得到这个负数对应的10进制值。这个数减去1等于11111111111111111111111111111011，再取一次反得到00000000000000000000000000000100，再加上负号就是-4。

// 可以简单记忆成，一个数与自身的取反值相加，等于-1。
~ -3 // 2

// 对一个整数连续两次二进制否运算，得到它自身。
~~3 // 3

// 使用二进制否运算取整，是所有取整方法中最快的一种。
~~1.999 // 1

// 对字符串进行二进制否运算，JavaScript 引擎会先调用Number函数，将字符串转为数值。
~'011'  // -12  // 相当于~Number('011')

// 对于其他类型的值，二进制否运算也是先用Number转为数值，然后再进行处理。
~[] // -1    // 相当于 ~Number([])
~NaN // -1   // 相当于 ~Number(NaN)
~null // -1  // 相当于 ~Number(null)
```
异或运算符（xor）：符号为^，表示若两个二进制位不相同，则结果为1，否则为0
```JavaScript
0 ^ 3 // 3    0（二进制00）与3（二进制11）进行异或运算，它们每一个二进制位都不同，所以得到11（即3）

// 异或运算 有一个特殊运用，连续对两个数a和b进行三次异或运算，可以互换它们的值。 这是互换两个变量的值的最快方法。
var a = 2, b = 3
a ^= b 
b ^= a 
a ^= b

a // 3
b // 2

// 异或运算也可以用来取整。
12.9 ^ 0 // 12
```
左移运算符（left shift）：符号为<<  ，表示将一个数的二进制值向左移动指定的位数，尾部补0，即乘以2的指定次方。
```JavaScript
4 << 1   // 8    4 的二进制形式为100，左移一位为1000（即十进制的8），相当于乘以2的1次方
-4 << 1  // -8  

// 如果左移0位，就相当于将该数值转为32位整数，等同于取整，对于正数和负数都有效。
13.5 << 0  // 13
-13.5 << 0  // -13

// 左移运算符用于二进制数值非常方便。
// 使用左移运算符，将颜色的 RGB 值转为 HEX 值

// RGB to HEX
// (1 << 24)的作用为保证结果是6位数
var rgb2hex = function(r, g, b) {
  return '#' + ((1 << 24) + (r << 16) + (g << 8) + b)
    .toString(16) // 先转成十六进制，然后返回字符串
    .substr(1);   // 去除字符串的最高位，返回后面六个字符串
}

var color = {r: 186, g: 218, b: 85};
rgb2hex(color.r, color.g, color.b)   // "#bada55"
```
右移运算符（right shift）：符号为>>，表示将一个数的二进制值向右移动指定的位数，头部补0，即除以2的指定次方（最高位即符号位不参与移动）
```JavaScript
4 >> 1   // 2   4的二进制形式为 00000000000000000000000000000100，右移一位得到00000000000000000000000000000010，即为十进制的2
-4 >> 1  // -2  -4的二进制形式为11111111111111111111111111111100，右移一位，头部补1，得到11111111111111111111111111111110,即为十进制的-2

// 右移运算可以模拟 2 的整除运算。
21 >> 3  // 2   相当于 21 / 8 = 2
```
带符号位的右移运算符（zero filled right shift）：符号为>>> ，表示将一个数的二进制形式向右移动，包括符号位也参与移动，头部补0。
所以，该运算总是得到正值。对于正数，该运算的结果与右移运算符（>>）完全一致，区别主要在于负数。
```JavaScript
4 >>> 1   // 2
-4 >>> 1  // 2147483646  将-4的二进制形式，带符号位的右移一位，得到01111111111111111111111111111110，即为十进制的2147483646
// 这个运算实际上将一个值转为32位无符号整数。
// 查看一个负整数在计算机内部的储存形式，最快的方法就是使用这个运算符。
-1 >>> 0 // 4294967295   
// -1作为32位整数时，内部的储存形式使用无符号整数格式解读，值为 4294967295，即(2^32)-1，等于11111111111111111111111111111111
```

#### void 运算符 
void运算符的作用是执行一个表达式，然后不返回任何值，或者说返回undefined。
```JavaScript
// void运算符的优先性很高，建议总是使用圆括号
void 0   // undefined
void(0)  // undefined   

// 这个运算符的主要用途是浏览器的书签工具（Bookmarklet），以及在超级链接中插入代码防止网页跳转。

<a href="http://example.com" onclick="f(); return false;">点击</a>   // 点击链接后，会先执行onclick的代码，由于onclick返回false，所以浏览器不会跳转到 example.com。
// void运算符可以取代上面的写法。
<a href="javascript: void(f())">文字</a>

// 下面是一个更实际的例子，用户点击链接提交表单，但是不产生页面跳转。
<a href="javascript: void(document.form.submit())">提交</a>
```

#### 逗号运算符
逗号运算符用于对两个表达式求值，并返回后一个表达式的值。逗号运算符的一个用途是，在返回一个值之前，进行一些辅助操作。
```JavaScript
var x = 0;
var y = (x++, 10);
x // 1
y // 10

var value = (console.log('Hi!'), true);  // Hi!
value // true
```

#### 运算顺序
* 运算符的优先级别（Operator Precedence）
![JS运算符的优先级](/images/expressions-operator-precedence.jpg)

* 圆括号的作用
圆括号不是运算符，而是一种语法结构，所以不具有求值作用。
共有两种用法：一种是把表达式放在圆括号之中，提升运算的优先级，它的优先级是最高的；跟在函数的后面，作用是调用函数。
```JavaScript
// 函数放在圆括号中，会返回函数本身。如果圆括号紧跟在函数的后面，就表示调用函数。
function f() {
  return 1;
}

(f) // function f(){return 1;}
f() // 1

// 圆括号之中，只能放置表达式，如果将语句放在圆括号之中，就会报错。
(var a = 1)   // SyntaxError: Unexpected token var
```
* 左结合与右结合 
对于优先级别相同的运算符，大多数情况，计算顺序总是从左到右，这叫做运算符的“左结合”（left-to-right associativity），即从左边开始计算。

少数运算符的计算顺序是从右到左，即从右边开始计算，这叫做运算符的“右结合”。 
一元运算符(逻辑非、按位非、一元加/减、前置递增/减、typeof、void、delete、await)、赋值运算符（=）、三元条件运算符（?:）和 指数运算符（**）、yield等
```JavaScript
w = x = y = z;   // 相当于 w = (x = (y = z));
q = a ? b : c ? d : e ? f : g;   // 相当于 q = a ? b : (c ? d : (e ? f : g));
2 ** 3 ** 2   // 512   相当于 2 ** (3 ** 2)
```

### 数据类型的转换
JavaScript 是一种动态类型语言，变量没有类型限制，可以随时赋予任意值。
#### 强制转换
强制转换主要指使用Number()、String()和Boolean()三个函数，手动将各种类型的值，分别转换成数字、字符串或者布尔值。
* Number()  可以将任意类型的值转化成数值。分两种情况，一种是参数是原始类型的值，另一种是参数是对象。
```JavaScript
// 原始类型值的转换规则如下:
// 数值：转换后还是原来的值
Number(324) // 324

// 字符串：如果可以被解析为数值，则转换为相应的数值
Number('324') // 324

// 字符串：如果不可以被解析为数值，返回 NaN
Number('324abc') // NaN

// 空字符串转为0
Number('') // 0

// 布尔值：true 转成 1，false 转成 0
Number(true) // 1
Number(false) // 0

// undefined：转成 NaN
Number(undefined) // NaN

// null：转成0
Number(null) // 0

// Number函数将字符串转为数值，要比parseInt函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为NaN。
parseInt('42 cats') // 42   parseInt逐个解析字符
Number('42 cats') // NaN    Number函数整体转换字符串的类型

// parseInt和Number函数都会自动过滤一个字符串前导和后缀的空格。
parseInt('\t\v\r12.34\n') // 12
Number('\t\v\r12.34\n') // 12.34

// 对象:
// 第一步，调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，不再进行后续步骤。
// 第二步，如果valueOf方法返回的还是对象，则改为调用对象自身的toString方法。如果toString方法返回原始类型的值，则对该值使用Number函数，不再进行后续步骤。
// 第三步，如果toString方法返回的是对象，就报错

//  Number方法的参数是对象时，将返回NaN，除非是包含单个数值的数组。
Number({a: 1}) // NaN
Number([1, 2, 3]) // NaN
Number([5]) // 5

// 默认情况下，对象的valueOf方法返回对象本身，所以一般总是会调用toString方法，而toString方法返回对象的类型字符串（比如[object Object]）
Number({}) // NaN

// valueOf和toString方法，都是可以自定义的。valueOf方法先于toString方法执行
Number({
  valueOf: function () {
    return 2;
  },
  toString: function () {
    return 3;
  }
})
```
* String()  将任意类型的值转化成字符串  分两种情况，一种是参数是原始类型的值，另一种是参数是对象。
```JavaScript
// 原始类型值:
// 数值：转为相应的字符串。
// 字符串：转换后还是原来的值。
// 布尔值：true转为字符串"true"，false转为字符串"false"。
// undefined：转为字符串"undefined"。
// null：转为字符串"null"
String(123) // "123"
String('abc') // "abc"
String(true) // "true"
String(undefined) // "undefined"
String(null) // "null"

// 对象:  
// String方法背后的转换规则，与Number方法基本相同，只是互换了valueOf方法和toString方法的执行顺序。
// 先调用对象自身的toString方法。如果返回原始类型的值，则对该值使用String函数，不再进行以下步骤。
// 如果toString方法返回的是对象，再调用原对象的valueOf方法。如果valueOf方法返回原始类型的值，则对该值使用String函数，不再进行以下步骤。
// 如果valueOf方法返回的是对象，就报错。

// String方法的参数如果是对象，返回一个类型字符串；如果是数组，返回该数组的字符串形式。
String({a: 1}) // "[object Object]"
String([1, 2, 3]) // "1,2,3"

// valueOf和toString方法，都是可以自定义的。toString方法先于valueOf方法执行
```
* Boolean()   将任意类型的值转为布尔值
```JavaScript
// 它的转换规则相对简单：除了undefined、null、-0或+0、NaN、''（空字符串）五个值的转换结果为false，其他的值全部为true。
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean(NaN) // false
Boolean('') // false

// 所有对象（包括空对象）的布尔值都是true，甚至连false对应的布尔对象new Boolean(false)也是true
Boolean({}) // true
Boolean([]) // true
Boolean(new Boolean(false)) // true
// 因为 JavaScript 语言设计的时候，出于性能的考虑，如果对象需要计算才能得到布尔值，对于obj1 && obj2这样的场景，可能会需要较多的计算。为了保证性能，就统一规定，对象的布尔值为true。
```
#### 自动转换
自动转换是以强制转换为基础的。遇到以下三种情况时，JavaScript 会自动转换数据类型，即转换是自动完成的，用户不可见。
```JavaScript
// 第一种情况，不同类型的数据互相运算。
123 + 'abc' // "123abc"

// 第二种情况，对非布尔值类型的数据求布尔值。
if ('abc') {console.log('true');}  // true

// 第三种情况，对非数值类型的值使用一元运算符（即+和-）。
+ {foo: 'bar'} // NaN
- [1, 2, 3] // NaN

// 自动转换的规则是这样的：预期什么类型的值，就调用该类型的转换函数。比如，某个位置预期为字符串，就调用String函数进行转换。如果该位置即可以是字符串，也可能是数值，那么默认转为数值。 由于自动转换具有不确定性，而且不易除错，建议在预期为布尔值、数值、字符串的地方，全部使用Boolean、Number和String函数进行显式转换。
// 自动转换为布尔值
// JavaScript 遇到预期为布尔值的地方（比如if语句的条件部分），就会将非布尔值的参数自动转换为布尔值。系统内部会自动调用Boolean函数。
// 除了 undefined、null、+0或-0、NaN、''（空字符串）五个值，其他都是自动转为true。
if ('abc') {console.log('true');}  // true
expression ? true : false
!! expression

// 自动转换为字符串
// JavaScript 遇到预期为字符串的地方，就会将非字符串的值自动转为字符串。具体规则是，先将复合类型的值转为原始类型的值，再将原始类型的值转为字符串。
// 字符串的自动转换，主要发生在字符串的加法运算时。当一个值为字符串，另一个值为非字符串，则后者转为字符串。
'5' + 1 // '51'
'5' + true // "5true"
'5' + {} // "5[object Object]"
'5' + [] // "5"
'5' + function (){} // "5function (){}"
'5' + undefined // "5undefined"
'5' + null // "5null"

// 自动转换为数值
// JavaScript 遇到预期为数值的地方，就会将参数值自动转换为数值。系统内部会自动调用Number函数。
// 除了加法运算符（+）有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值。
'5' - '2' // 3
true - 1  // 0
'5' * []    // 0
false / '5' // 0
'abc' - 1   // NaN
// null转为数值时为0，而undefined转为数值时为NaN
null + 1 // 1
undefined + 1 // NaN
// 一元运算符也会把运算子转成数值。
+'abc' // NaN
-'abc' // NaN
+true // 1
-false // 0
```

### 错误处理机制
#### Error 实例对象
JavaScript 解析或运行时，一旦发生错误，引擎就会抛出一个错误对象。JavaScript 原生提供Error构造函数，所有抛出的错误都是这个构造函数的实例。
```JavaScript
var err = new Error('出错了');
err.message // "出错了"

// message：错误提示信息。 必须有message属性
// name：错误名称（非标准属性）
// stack：错误的堆栈（非标准属性）

function throwit() {
  throw new Error('');
}
function catchit() {
  try {
    throwit();
  } catch(e) {
    console.log(e.stack); // print stack trace
    // Error
    //    at throwit (~/examples/throwcatch.js:9:11)
    //    at catchit (~/examples/throwcatch.js:3:9)
    //    at repl:1:5
    // 上面代码中，错误堆栈的最内层是throwit函数，然后是catchit函数，最后是函数的运行环境
  }
}

catchit()
```
#### 原生错误类型
Error实例对象是最一般的错误类型，在它的基础上，JavaScript 还定义了其他6种错误对象。也就是说，存在Error的6个派生对象。
* SyntaxError 对象   
解析代码时发生的语法错误。当Javascript语言解析代码时,Javascript引擎发现了不符合语法规范的token或token顺序时抛出SyntaxError.
new SyntaxError([message[, fileName[, lineNumber]]])

* ReferenceError 对象  
引用一个不存在的变量时发生的错误，或者将一个值分配给无法分配的对象，比如对函数的运行结果或者this赋值

* RangeError 对象  
一个值超出有效范围时发生的错误。主要有几种情况，一是数组长度为负数，二是Number对象的方法参数超出范围，以及函数堆栈超过最大值。

* TypeError 对象
TypeError对象是变量或参数不是预期类型时发生的错误。比如，对字符串、布尔值、数值等原始类型的值使用new命令，就会抛出这种错误，因为new命令的参数应该是一个构造函数。 调用对象不存在的方法，也会抛出TypeError错误，因为obj.unknownMethod的值是undefined，而不是一个函数。

* URIError 对象
URI 相关函数的参数不正确时抛出的错误，主要涉及encodeURI()、decodeURI()、encodeURIComponent()、decodeURIComponent()、escape()和unescape()这六个函数。

* EvalError 对象
eval函数没有被正确执行时，会抛出EvalError错误。该错误类型已经不再使用了，只是为了保证与以前代码兼容，才继续保留。

* 自定义错误
除了 JavaScript 原生提供的七种错误对象，还可以定义自己的错误对象。
```JavaScript
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

UserError.prototype = new Error();
UserError.prototype.constructor = UserError;
// 上面代码自定义一个错误对象UserError，让它继承Error对象。然后，就可以生成这种自定义类型的错误了。
new UserError('这是自定义的错误！');
```
* throw 语句
手动中断程序执行，抛出一个错误。throw可以抛出任何类型的值。也就是说，它的参数可以是任何值。
遇到throw语句，程序就中止了。引擎会接收到throw抛出的信息，可能是一个错误实例，也可能是其他类型的值。

* try...catch...finally
一旦发生错误，程序就中止执行了。JavaScript 提供了try...catch结构，允许对错误进行处理，选择是否往下执行。
```JavaScript
try {
  throw new Error('出错了');
} catch(e) {  // catch接受一个参数，表示try代码块抛出的值
  // 捕获错误，处理错误。 catch代码块捕获错误之后，程序不会中断，会按照正常流程继续执行下去
    if (e instanceof SyntaxError) {
      console.log(e.message)
    } else if (e instanceof RangeError) {
      console.log(e.message)
    }
} finally {
  // 允许在最后添加一个finally代码块，表示不管是否出现错误，都必需在最后运行的语句
  console.log('完成了');
}
```

### 编程风格
“编程风格”（programming style）指的是编写代码的样式规则，要考虑如何尽量使代码清晰易读、减少出错，风格一致。
主要涉及缩进、区块、圆括号、行尾的分号、全局变量、变量声明、相等和严格相等、语句的合并、自增和自减运算符等。
* switch...case 结构 
建议switch...case结构可以用对象结构代替。switch...case结构要求，在每一个case的最后一行必须是break语句，否则会接着运行下一个case。这样不仅容易忘记，还会造成代码的冗长。而且，switch...case不使用大括号，不利于代码形式的统一。此外，这种结构类似于goto语句，容易造成程序流程的混乱，使得代码结构混乱不堪，不符合面向对象编程的原则。
```JavaScript
function doAction(action) {
  switch (action) {
    case 'hack':
      return 'hack';
    case 'slash':
      return 'slash';
    case 'run':
      return 'run';
    default:
      throw new Error('Invalid action.');
  }
}

// 建议改写成对象结构
function doAction(action) {
  var actions = {
    'hack': function () {
      return 'hack';
    },
    'slash': function () {
      return 'slash';
    },
    'run': function () {
      return 'run';
    }
  };

  if (typeof actions[action] !== 'function') {
    throw new Error('Invalid action.');
  }

  return actions[action]();
}
```

### console 对象与控制台
console对象是 JavaScript 的原生对象，类似 Unix 系统的标准输出stdout和标准错误stderr，可以输出各种信息到控制台，并且还提供了很多有用的辅助方法。
打开控制台: 按 F12 或者Control + Shift + i（PC）/ Command + Option + i（Mac）。

#### console 对象的静态方法
* console.log()
console.log方法用于在控制台输出信息。
```JavaScript
// 如果第一个参数是格式字符串（使用了格式占位符），console.log方法将依次用后面的参数替换占位符，然后再进行输出。
console.log(' %s + %s = %s', 1, 1, 2)  //  1 + 1 = 2

// 它可以接受一个或多个参数，将它们连接起来输出。会自动在每次输出的结尾，添加换行符。
console.log('Hello World') // Hello World
console.log('a', 'b', 'c')  // a b c

// 如果第一个参数是格式字符串（使用了格式占位符），console.log方法将依次用后面的参数替换占位符，然后再进行输出。
console.log(' %s + %s = %s', 1, 1, 2)  //  1 + 1 = 2
// 支持以下占位符: %s 字符串、%d 整数、%i 整数、%f 浮点数、%o 对象的链接、%c CSS 格式字符串。不同类型的数据必须使用对应的占位符
var number = 11 * 9;
var color = 'red';
console.log('%d %s balloons', number, color);  // 99 red balloons

// 使用%c占位符时，对应的参数必须是 CSS 代码，用来对输出内容进行 CSS 渲染。
console.log('%cThis text is styled!', 'color: red; background: yellow; font-size: 24px;')   // 输出的内容将显示为黄底红字

// 如果参数是一个对象，console.log会显示该对象的值。
console.log({foo: 'bar'})  // {foo: "bar"}
console.log(Date)  // function Date() { [native code] }    输出Date对象的值，结果为一个构造函数

// console对象的所有方法，都可以被覆盖。因此，可以按照自己的需要，定义console.log方法。
['log', 'info', 'warn', 'error'].forEach(function(method) {
  console[method] = console[method].bind(
    console,
    new Date().toISOString()
  );
});

console.log("出错了！");   // 2019-03-02T11:01:37.745Z 出错了！
```
* console.table()
对于某些复合类型的数据，console.table方法可以将其转为表格显示。
```JavaScript
var languages = [
  { name: "JavaScript", fileExtension: ".js" },
  { name: "TypeScript", fileExtension: ".ts" },
  { name: "CoffeeScript", fileExtension: ".coffee" }
];

console.table(languages);
```
* console.count()
count方法用于计数，输出它被调用了多少次。
```JavaScript
function greet(user) {
  console.count();
  return 'hi ' + user;
}
// 每次调用greet函数，内部的console.count方法就输出执行次数。
greet('bob')     // default: 1   // "hi bob"
greet('alice')   // default: 2   // "hi alice"

// 该方法可以接受一个字符串作为参数，作为标签，对执行次数进行分类。
function greet(user) {
  console.count(user);
  return "hi " + user;
}

greet('bob')  // bob: 1   // "hi bob"
greet('bob')  // bob: 2   // "hi bob"
```
* console.dir()、console.dirxml() 
dir方法用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。
```JavaScript
console.log({f1: 'foo', f2: 'bar'})   // {f1: "foo", f2: "bar"}
console.dir({f1: 'foo', f2: 'bar'})   // Object  f1: "foo"   f2: "bar"__proto__: Object

// 该方法对于输出 DOM 对象非常有用，因为会显示 DOM 对象的所有属性。
console.dir(document.body)

// Node 环境之中，还可以指定以代码高亮的形式输出。
console.dir(obj, {colors: true})

// dirxml方法主要用于以目录树的形式，显示 DOM 节点。
console.dirxml(document.body)

// 如果参数不是 DOM 节点，而是普通的 JavaScript 对象，console.dirxml等同于console.dir。
console.dirxml([1, 2, 3])
// 等同于
console.dir([1, 2, 3])
```
* console.assert()
主要用于程序运行过程中，进行条件判断，如果不满足条件，就显示一个错误，但不会中断程序执行。这样就相当于提示用户，内部状态不正确。
它接受两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会提示有错误，在控制台输出第二个参数，否则不会有任何结果。
```JavaScript
console.assert(false, '判断条件不成立')  // Assertion failed: 判断条件不成立

// 相当于
try {
  if (!false) {
    throw new Error('判断条件不成立');
  }
} catch(e) {
  console.error(e);
}

// 如，判断子节点的个数是否大于等于500。如果符合条件的节点小于500个，不会有任何输出；只有大于等于500时，才会在控制台提示错误，并且显示指定文本。
console.assert(list.childNodes.length < 500, '节点个数大于等于500') 
```
* console.time()，console.timeEnd()
用于计时，可以算出一个操作所花费的准确时间。time方法表示计时开始，timeEnd方法表示计时结束。它们的参数是计时器的名称。调用timeEnd方法之后，控制台会显示“计时器名称: 所耗费的时间”。
```JavaScript
console.time('Array initialize');

var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
  array[i] = new Object();
};

console.timeEnd('Array initialize');    // Array initialize: 339.818115234375ms
```
* console.group()，console.groupEnd()，console.groupCollapsed()
console.group和console.groupEnd这两个方法用于将显示的信息分组。它只在输出大量信息时有用，分在一组的信息，可以用鼠标折叠/展开。
```JavaScript
// 下面代码会将“二级分组”显示在“一级分组”内部，并且“一级分组”和“二级分组”前面都有一个折叠符号，可以用来折叠本级的内容。
console.group('一级分组');
console.log('一级分组的内容');

console.group('二级分组');
console.log('二级分组的内容');

console.groupEnd(); // 二级分组结束
console.groupEnd(); // 一级分组结束
```
console.groupCollapsed方法与console.group方法很类似，唯一的区别是该组的内容，在第一次显示时是收起的（collapsed），而不是展开的。

* console.trace()
显示当前执行的代码在堆栈中的调用路径
```JavaScript
console.trace()

// Trace
//     at repl:1:9
//     at ContextifyScript.Script.runInThisContext (vm.js:50:33)
//     ...
```
* console.clear()
用于清除当前控制台的所有输出，将光标回置到第一行。如果用户选中了控制台的“Preserve log”选项，console.clear方法将不起作用。


#### 控制台命令行 API
* $_   返回上一个表达式的值
* $0 - $4   控制台保存了最近5个在 Elements 面板选中的 DOM 元素，$0代表倒数第一个（最近一个），$1代表倒数第二个，以此类推直到$4。
* $(selector)   返回第一个匹配的元素，等同于document.querySelector()
* $$(selector)  返回选中的 DOM 对象，等同于document.querySelectorAll
* inspect(window)  打开相关面板，并选中相应的元素，显示它的细节。 比如inspect(document)会在 Elements 面板显示document元素
* getEventListeners(object) 返回一个对象，该对象的成员为object登记了回调函数的各种事件（比如click或keydown），每个事件对应一个数组，数组的成员为该事件的回调函数
* keys(object)  返回一个数组，包含object的所有键名
* values(object)  返回一个数组，包含object的所有键值
* （9）monitorEvents(object[, events]) 监听特定对象上发生的特定事件。事件发生时，会返回一个Event对象，包含该事件的相关信息
* unmonitorEvents(object[, events])  用于停止监听   如monitorEvents(window, ["resize", "scroll"]) 
```JavaScript
// 下面代码分别表示单个事件和多个事件的监听方法。
monitorEvents(window, ["resize", "scroll"])
monitorEvents(window, "resize");
// 下面代码表示如何停止监听
monitorEvents($0, 'mouse');
unmonitorEvents($0, 'mousemove');

// monitorEvents允许监听同一大类的事件。所有事件可以分成四个大类。
mouse："mousedown", "mouseup", "click", "dblclick", "mousemove", "mouseover", "mouseout", "mousewheel"
key："keydown", "keyup", "keypress", "textInput"
touch："touchstart", "touchmove", "touchend", "touchcancel"
control："resize", "scroll", "zoom", "focus", "blur", "select", "change", "submit", "reset"

// 监听所有key大类的事件
monitorEvents($("#msg"), "key");

```
* clear()：清除控制台的历史
* copy(object)：复制特定 DOM 元素到剪贴板
* dir(object)：显示特定对象的所有属性，是console.dir方法的别名

#### debugger 语句
debugger语句主要用于除错，作用是设置断点。如果有正在运行的除错工具，程序运行到debugger语句时会自动停下。如果没有除错工具，debugger语句不会产生任何结果，JavaScript 引擎自动跳过这一句。

Chrome 浏览器中，当代码运行到debugger语句时，就会暂停运行，自动打开脚本源码界面。


###  标准库
#### Object 对象
JavaScript 原生提供Object对象,JavaScript 的所有其他对象都继承自Object对象，即那些对象都是Object的实例。

Object对象的原生方法分成两类：Object本身的方法与Object的实例方法。

```JavaScript
// Object对象本身的方法,即直接定义在Object对象的方法。
Object.print = function (o) { console.log(o) };

// Object的实例方法，即定义在Object原型对象Object.prototype上的方法。它可以被Object实例直接使用
// 凡是定义在Object.prototype对象上面的属性和方法，将被所有实例对象共享
Object.prototype.print = function () {
  console.log(this);
};

var obj = new Object();
obj.print() // {}
```
##### Object() 工具方法
Object本身是一个函数，可以当作工具方法使用，将任意值转为对象。这个方法常用于保证某个值一定是对象。
```JavaScript
// 如果参数为空（或者为undefined和null），Object()返回一个空对象。
var obj = Object();
// 等同于
var obj = Object(undefined);    // 将undefined和null转为对象，结果得到了一个空对象obj
var obj = Object(null);
obj instanceof Object // true

// 如果参数是原始类型的值，Object方法将其转为对应的包装对象的实例(原始类型值对应的包装对象)
var obj = Object(1);
obj instanceof Object // true
obj instanceof Number // true

// 如果Object方法的参数是一个对象，它总是返回该对象，即不用转换。
var arr = [];
var obj = Object(arr); // 返回原数组
obj === arr // true

// 利用这一点，可以写一个判断变量是否为对象的函数。
function isObject(value) {
  return value === Object(value);
}

isObject([]) // true
isObject(true) // false
```
#####  new Object() 构造函数
Object不仅可以当作工具函数使用，还可以当作构造函数使用，即前面可以使用new命令。
```JavaScript
// Object构造函数的首要用途，是直接通过它来生成新对象。
var obj = new Object();
// 等同于
var obj = {}

// 可以接受一个参数，如果该参数是一个对象，则直接返回这个对象；如果是一个原始类型的值，则返回该值对应的包装对象
var o1 = {a: 1}; 
var o2 = new Object(o1);
o1 === o2 // true

// Object构造函数的用法与工具方法虽然用法相似,但语义不同，Object(value)表示将value转成一个对象，new Object(value)则表示新生成一个对象，它的值是value
```
#####  Object 的静态方法
* Object.keys()，Object.getOwnPropertyNames()
由于 JavaScript 没有提供计算对象属性个数的方法，所以可以用这两个方法代替。如Object.keys(obj).length 
Object.keys方法的参数是一个对象，返回一个数组。该数组的成员都是该对象自身的（而不是继承的）可枚举的属性名。
Object.getOwnPropertyNames方法与Object.keys类似，也是接受一个对象作为参数，返回一个数组，包含了该对象自身的所有(含不可枚举)属性名。
* 对象属性模型的相关方法
Object.getOwnPropertyDescriptor()：获取某个属性的描述对象。
Object.defineProperty()：通过描述对象，定义某个属性。
Object.defineProperties()：通过描述对象，定义多个属性。
* 控制对象状态的方法
Object.preventExtensions()：防止对象扩展。
Object.isExtensible()：判断对象是否可扩展。
Object.seal()：禁止对象配置。
Object.isSealed()：判断一个对象是否可配置。
Object.freeze()：冻结一个对象。
Object.isFrozen()：判断一个对象是否被冻结
* 原型链相关方法
Object.create()：该方法可以指定原型对象和属性，返回一个新的对象。
Object.getPrototypeOf()：获取对象的Prototype对象。

##### Object 的实例方法
除了静态方法，还有不少方法定义在Object.prototype对象。它们称为实例方法，所有Object的实例对象都继承了这些方法。
* Object.prototype.valueOf()：返回当前对象对应的值。默认情况下返回对象本身。自动类型转换时会默认调用这个方法
```JavaScript
var obj = new Object();
1 + obj // "1[object Object]"    默认调用valueOf()方法，求出obj的值再与1相加。可自定义valueOf方法
```
* Object.prototype.toString()：返回当前对象对应的字符串形式。默认情况下返回类型字符串。
```JavaScript
// 对于一个对象调用toString方法，会返回字符串[object Object]，该字符串说明对象的类型
var o1 = new Object();
o1.toString() // "[object Object]"

var o2 = {a:1};
o2.toString() // "[object Object]"

// 数组、字符串、函数、Date 对象都分别部署了自定义的toString方法，覆盖了Object.prototype.toString原始方法。
[1, 2, 3].toString() // "1,2,3"
'123'.toString() // "123"

(function () {
  return 123;
}).toString()
// "function () {
//   return 123;
// }"

(new Date()).toString()  // "Sun Mar 03 2019 13:11:11 GMT+0800 (China Standard Time)"


// Object.prototype.toString方法返回对象的类型字符串，是一个十分有用的判断数据类型的方法。 不使用可能被覆盖的obj.toString()自定义方法。
Object.prototype.toString.call(value)
// 不同数据类型的Object.prototype.toString方法返回值如下。
数值：返回[object Number]。
字符串：返回[object String]。
布尔值：返回[object Boolean]。
undefined：返回[object Undefined]。
null：返回[object Null]。
数组：返回[object Array]。
arguments 对象：返回[object Arguments]。
函数：返回[object Function]。
Error 对象：返回[object Error]。
Math 对象：返回[object Math]。
Date 对象：返回[object Date]。
RegExp 对象：返回[object RegExp]。
其他对象：返回[object Object]。

// 利用这个特性，可以写出一个比typeof运算符更准确的类型判断函数。
var type = function (o){
  var s = Object.prototype.toString.call(o);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};

type({}); // "object"

// 在上面这个type函数的基础上，还可以加上专门判断某种类型数据的方法。
['Null', 'Undefined', 'Object', 'Array', 'String', 'Number', 'Boolean', 'Function', 'RegExp'].forEach(function (t) {
  type['is' + t] = function (o) {
    return type(o) === t.toLowerCase();
  };
});

type.isRegExp(/abc/) // true
```
* Object.prototype.toLocaleString()：返回当前对象对应的本地字符串形式。
* Object.prototype.hasOwnProperty()：判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性。
* Object.prototype.isPrototypeOf()：判断当前对象是否为另一个对象的原型。
* Object.prototype.propertyIsEnumerable()：判断某个属性是否可枚举。


#### 属性描述对象
JavaScript 提供了一个内部数据结构，用来描述对象的属性，控制它的行为，比如该属性是否可写、可遍历等等。这个内部数据结构称为“属性描述对象”（attributes object）。每个属性都有自己对应的属性描述对象，保存该属性的一些元信息。
```JavaScript
// 属性描述对象提供6个元属性
{
  value: undefined,  // 目标属性的值，默认为 undefined
  writable: true,    // 是否可写，默认为 true
  enumerable: true,  // 是否可枚举，默认为true   
  // 有四个操作会忽略enumerable为false的属性
    // for...in循环：只遍历对象自身的和继承的可枚举的属性。
    // Object.keys()：返回对象自身的所有可枚举的属性的键名。
    // JSON.stringify()：只串行化对象自身的可枚举的属性。
    // Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性
  configurable: true,   // 是否可配置性，默认为true
  // 若为false，将阻止某些操作改写该属性，比如无法删除，也不得改变该属性的属性描述对象（value除外）,即configurable属性控制了属性描述对象的可写性
  get: undefined,  // 该属性的取值函数（getter），默认为undefined
  set: undefined   // 该属性的存值函数（setter），默认为undefined
}


// JavaScript 还提供了存取器的另一种写法。这种写法与上面定义属性描述对象是等价的，而且使用更广泛。
var obj = {
  get p() {
    return 'getter';
  },
  set p(value) {
    console.log('setter: ' + value);
  }
};

// 存取器往往用于，属性的值依赖对象内部数据的场合。
var obj ={
  $n : 5,
  get next() { return this.$n++ },
  set next(n) {
    if (n >= this.$n) this.$n = n;
    else throw new Error('新的值必须大于当前值');
  }
};

obj.next // 5    next属性的存值函数和取值函数，都依赖于内部属性$n。
obj.next = 10;
obj.next // 10
obj.next = 5;  // Uncaught Error: 新的值必须大于当前值
```
* Object.getOwnPropertyDescriptor()
可以获取自身属性描述对象，不能用于继承的属性。它的第一个参数是目标对象，第二个参数是一个字符串，对应目标对象的某个属性名。
```JavaScript
var obj = { p: 'a' };
Object.getOwnPropertyDescriptor(obj, 'p')  // { value: "a", writable: true, enumerable: true, configurable: true } 
```
* Object.getOwnPropertyNames()
返回一个数组，成员是参数对象自身的全部属性的属性名，不管该属性是否可遍历。
```JavaScript
Object.getOwnPropertyNames([])   // [ 'length' ]
Object.getOwnPropertyNames(Object.prototype)   
// ["constructor", "__defineGetter__", "__defineSetter__", "hasOwnProperty", "__lookupGetter__", "__lookupSetter__", "isPrototypeOf", "propertyIsEnumerable", "toString", "valueOf", "__proto__", "toLocaleString"]
```
* Object.defineProperty(object, propertyName, attributesObject)
允许通过属性描述对象，定义或修改一个属性，然后返回修改后的对象
```JavaScript
// value默认为undefined，writable、configurable、enumerable默认为false
var obj = Object.defineProperty({}, 'p', {
  value: undefined,
  writable: false,
  enumerable: false,
  configurable: false
});

// 如果一次性定义或修改多个属性，可以使用Object.defineProperties()方法。
var obj2 = Object.defineProperties({}, {
  p1: { value: 123, enumerable: true },
  p2: { value: 'abc', enumerable: true },
  p3: { get: function () { return this.p1 + this.p2 }, enumerable:true, configurable:true }
});

// 注意，如果一个描述符同时有(get或set) 和 (value或writable)关键字，将会产生一个异常
```
* Object.prototype.propertyIsEnumerable()
返回一个布尔值，用来判断某个属性是否可遍历。注意，这个方法只能用于判断对象自身的属性，对于继承的属性一律返回false。 
如obj.propertyIsEnumerable('toString') // false

* 对象的拷贝
有时，我们需要将一个对象的所有属性，拷贝到另一个对象，可以用下面的方法实现。
```JavaScript
var extend = function (to, from) {
  for (var property in from) {
    if (!from.hasOwnProperty(property)) continue;
    Object.defineProperty(to, property, Object.getOwnPropertyDescriptor(from, property));
  }
  return to;
}

extend({}, { get a(){ return 1 } })  // { get a(){ return 1 } })
```
* 控制对象状态
有时需要冻结对象的读写状态，防止对象被改变。JavaScript 提供了三种冻结方法，
最弱的一种是Object.preventExtensions，其次是Object.seal，最强的是Object.freeze。

Object.preventExtensions() 无法再添加新的属性。 用 Object.isExtensible()检测
Object.seal() 禁止新增或删除旧属性，并不影响修改某个属性的值。实质是把属性描述对象的configurable属性设为false。 用 Object.isSealed()检测
Object.freeze() 无法添加新属性、无法删除旧属性、也无法改变属性的值，使得这个对象实际上变成了常量。 用 Object.isFrozen()检测

```JavaScript
// 上面的三个方法锁定对象的可写性有一个漏洞：可以通过改变原型对象，来为对象增加属性。一种解决方案是，把obj的原型也冻结住。
var obj = new Object();
Object.preventExtensions(obj);

var proto = Object.getPrototypeOf(obj);
Object.preventExtensions(proto);

proto.t = 'hello';
obj.t // undefined

// 另外一个局限是，如果属性值是对象，上面这些方法只能冻结属性指向的对象，而不能冻结对象本身的内容。
var obj = {
  foo: 1,
  bar: ['a', 'b']
};
Object.freeze(obj);

obj.bar.push('c');
obj.bar // ["a", "b", "c"]   obj.bar属性指向一个数组，obj对象被冻结以后，这个指向无法改变，即无法指向其他值，但是所指向的数组是可以改变的。
```

#### Array 对象
Array是 JavaScript 的原生对象，同时也是一个构造函数，可以用它生成新的数组。
##### 构造函数 new Array()
```JavaScript
var arr = new Array(2);   // [ empty x 2 ] 生成一个两个成员的数组，每个位置都是空值
// 如果没有使用new，运行结果也是一样的。等同于
var arr = Array(2);

// Array构造函数有一个很大的缺陷，就是不同的参数，会导致它的行为不一致。
// 无参数时，返回一个空数组
new Array() // []
// 单个正整数参数，表示返回的新数组的长度
new Array(2) // [ empty x 2 ]
// 非正整数的数值作为参数，会报错
new Array(3.2) // RangeError: Invalid array length
new Array(-3) // RangeError: Invalid array length
// 单个非数值（比如字符串、布尔值、对象等）作为参数，则该参数是返回的新数组的成员
new Array('abc') // ['abc']
new Array([1]) // [Array[1]]
// 多参数时，所有参数都是返回的新数组的成员
new Array(1, 2) // [1, 2]
new Array('a', 'b', 'c') // ['a', 'b', 'c']

// 因此，不建议使用它生成新数组，直接使用数组字面量是更好的做法
var arr = new Array(1, 2); // bad
var arr = [1, 2];  // good

// 如果参数是一个正整数，返回数组的成员都是空位。虽然读取的时候返回undefined，但实际上该位置没有任何值。虽然可以取到length属性，但是取不到键名。
var a = new Array(2);  // [empty × 2]
var b = [undefined, undefined];  // [undefined, undefined]
```
#####  静态方法
Array.isArray() 方法返回一个布尔值，表示参数是否为数组。它可以弥补 typeof 运算符的不足。
```JavaScript
// typeof 运算符只能显示数组的类型是Object，而Array.isArray() 方法可以识别数组。
var arr = [1, 2, 3];
typeof arr // "object"
Array.isArray(arr) // true
```
#####  实例方法
* valueOf() 方法是一个所有对象都拥有的方法，表示对该对象求值。不同对象的valueOf方法不尽一致，数组的valueOf方法返回数组本身。
* toString() 方法也是对象的通用方法，数组的toString方法返回数组的字符串形式。
```JavaScript
var arr = [1, 2, 3];
arr.valueOf() // [1, 2, 3]


var arr = [1, 2, 3];
arr.toString() // "1,2,3"

var arr = [1, 2, 3, [4, 5, 6]];
arr.toString() // "1,2,3,4,5,6"
```
* push() 方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。
* pop() 方法用于删除数组的最后一个元素，并返回该元素。注意，该方法会改变原数组。
```JavaScript
// push和pop结合使用，就构成了 LIFO（Last-In-First-Out，后进先出）的栈结构（stack）。最新添加的项最早被移除。比如，箱子里的书，后放入的先取出。
arr.push('d') // 4
arr // ["a", "b", "c", "d"]

arr.pop() // 'd'   
arr  // ["a", "b", "c"]   'd'是最后进入数组的，但是最早离开数组

// 对空数组使用pop方法，不会报错，而是返回undefined。
[].pop() // undefined
```
* shift()方法用于删除数组的第一个元素，并返回该元素。注意，该方法会改变原数组。
* unshift()方法用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。
```JavaScript
var arr = ['a', 'b', 'c'];
arr.shift()  // 'a'
arr  // ['b', 'c']

arr.unshift('x');  // 3
arr  // ["x", "b", "c"]

// unshift()方法可以接受多个参数，这些参数都会添加到目标数组头部。
arr.unshift('1', '2')  // 5
arr  // ["1", "2", "x", "b", "c"]

// push和shift结合使用，就构成了 FIFO(Fist-In-First-Out,先进先出) 队列结构（queue）。即在数组的后端添加项，从数组的前端移除项。比如，火车站排队买票，先到的先买，买好的先走。
```
* join()方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。如果不提供参数，默认用逗号分隔。
```JavaScript
var a = [1, 2, 3, 4];

a.join() // "1,2,3,4"
a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"

// 如果数组成员是undefined或null或空位，会被转成空字符串。
[undefined, , null].join('#')  // '##'

// 通过call方法，这个方法也可以用于字符串或类似数组的对象。
var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')  // "a-b"
Array.prototype.join.call('hello', '-')  // "h-e-l-l-o"
```
* concat() 方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组，原数组不变。
```JavaScript
['hello'].concat(['world'], ['!'])   // ["hello", "world", "!"]
[].concat({a: 1}, {b: 2})  // [{ a: 1 }, { b: 2 }]

// 除了数组作为参数，concat也接受其他类型的值作为参数，添加到目标数组尾部。
[1, 2, 3].concat(4, 5, 6)   // [1, 2, 3, 4, 5, 6]

// 如果数组成员包括对象，concat方法返回当前数组的一个浅拷贝。所谓“浅拷贝”，指的是新数组拷贝的是对象的引用，改变原对象以后，新数组跟着改变。
var obj = { a: 1 };
var oldArray = [obj];
var newArray = oldArray.concat();
obj.a = 2;
newArray[0].a // 2
```
* reverse() 方法用于颠倒排列数组元素，返回改变后的数组。注意，该方法将改变原数组。
* slice() 方法用于提取目标数组的一部分，返回一个新数组，它是一个由 start和 end（不包括end）决定的原数组的浅拷贝。原始数组不会被改变。
```JavaScript
// 它的第一个参数为起始位置（从0开始），第二个参数为终止位置（不含）。如果省略第二个参数，则一直返回到原数组的最后一个成员。
arr.slice(start,end)

// 如果slice没有参数，实际上等于返回一个原数组的拷贝。
// 如果slice方法的参数是负数，则表示倒数计算的位置。
// 如果第一个参数大于等于数组长度，或者第二个参数小于第一个参数，则返回空数组。

// slice方法的一个重要应用，是将类似数组的对象转为真正的数组。
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })  // ['a', 'b']
Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```
* splice方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。注意，该方法会改变原数组。
```JavaScript
arr.splice(index,howmany,item1,.....,itemX)
// index 必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
// howmany	必需。要删除的项目数量。如果设置为 0，则不会删除项目。
// item1, ..., itemX	可选。向数组添加的新项目。

// 下面代码从原数组2号位置，删除了两个数组成员，还插入了两个新成员。
var a = ['a', 'b', 'c', 'd'];
a.splice(2, 2, 1, 2) // ["c", "d"]
a // ["a", "b", 1, 2]

// 如果只是单纯地插入元素，splice方法的第二个参数可以设为0。
var b = [1, 1, 1];
b.splice(1, 0, 2) // []
b // [1, 2, 1, 1]

// 如果只提供第一个参数，等同于将原数组在指定位置拆分成两个数组。
var c = [1, 2, 3, 4];
c.splice(2) // [3, 4]
c // [1, 2]
```
* sort() 方法对数组成员进行排序，默认是按照字典顺序排序。排序后，原数组将被改变。
```JavaScript
arr.sort([compareFunction])

// sort方法不是按照大小排序，而是按照字典顺序。也就是说，数值会被先转成字符串，再按照字符串Unicode码点进行比较
['d', 'c', 'b', 'a'].sort()  // ['a', 'b', 'c', 'd']
[8, 7, 9].sort()  // [7, 8, 9]
[10111, 1101, 111].sort()  // [10111, 1101, 111]

// 如果让sort方法按照自定义方式排序，可以传入一个函数作为参数。接受两个参数，表示进行比较的两个数组成员。
// 如果返回值小于 0 ，那么 a 会被排列到 b 之前；
// 如果返回值等于 0 ，那么  a 和 b 的相对位置不变；
// 如果返回值大于 0 ，那么 b 会被排列到 a 之前。

// 要比较数字而非字符串，比较函数可以简单的以 a和b相减。a-b升序，b-a降序。
[8, 7, 9].sort(function (a, b) {
  //a-b升序，b-a降序
  return a - b;
})
// [7, 8, 9]
```
* map() 方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回，原数组没有变化。
```JavaScript
var new_arr = array.map(function(currentValue, index, arr), thisValue)
// map方法接受一个函数作为参数，创建一个新数组。该函数调用时，map方法向它传入三个参数：当前成员currentValue、当前位置index和数组本身arr。 
// 此外还可以接受第二个参数thisValue用来绑定回调函数内部的this。如果省略了 thisValue，或者传入 null、undefined，那么回调函数的 this 为全局对象。
var arr = [1, 2, 3];
arr.map(function (ele) {
  return ele + 1;
});
// [2, 3, 4]
arr  // [1, 2, 3]


// 下面代码通过map方法的第二个参数，将回调函数内部的this对象，指向arr数组。
var arr2 = ['a', 'b', 'c'];
[1, 2].map(function (e) {
  return this[e];
}, arr2)
// ['b', 'c']

// 如果数组有空位，map方法的回调函数在这个位置不会执行，会跳过数组的空位。map方法不会跳过undefined和null，但是会跳过空位。
var f = function (n) { return 'a' };
[1,undefined, null,,2].map(f) // ["a", "a", "a", empty, "a"]
```
* forEach() 方法 对数组的所有成员依次执行参数函数。和map很相似，
```JavaScript
array.forEach(function(currentValue, index, arr), thisValue)

// 但是，forEach方法不像 map() 或者 reduce()，它总是返回undefined值，只用来操作数据，并且不可链式调用(在一个链的最后执行副作用)。这就是说，如果数组遍历的目的是为了得到返回值，那么使用map方法，否则使用forEach方法。
['a', 'b', 'c'].forEach(function(ele) {
  console.log(ele);
});
// a b c

// forEach方法也可以接受第二个参数，绑定参数函数的this变量。注意，如果使用箭头函数，thisArg 参数会被忽略，因为箭头函数在词法上绑定了 this 值。
// forEach方法也不会跳过undefined和null，但是会跳过空位。
var out = [];
[1, 2, ,3].forEach(function(elem) {
  this.push(elem * elem);
}, out);

out // (5) [1, 4, 0, NaN, 9] 注意索引2被跳过了，因为在数组的这个位置是空位

// 注意，forEach方法无法中断执行或跳出循环，总是会将所有成员遍历完，除了抛出一个异常。若你需要提前终止循环，你可以使用for、for...of、every、some、find、findIndex，他们可以对数组元素判断，以便确定是否需要继续遍历。或者也可以使用 filter() 提前过滤出需要遍历的部分，再用 forEach() 处理。
```
* filter() 方法用于过滤数组成员，满足条件的成员组成一个新数组返回。如果没有通过测试则返回空数组。
```JavaScript
var new_array = array.filter(function(currentValue, index, arr), thisArg])

// 它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。
[1, 2, 3, 4, 5].filter(function (ele) {
  return (ele > 3);
})
// [4, 5]

// filter方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组。
[1, 2, 3, 4, 5].filter(function (ele, index, arr) {
  return index % 2 === 0;
});
// [1, 3, 5]

// filter方法还可以接受第二个参数，用来绑定参数函数内部的this变量。
var obj = { MAX: 3 };
var myFilter = function (item) {
  if (item > this.MAX) return true;  // 过滤器myFilter内部有this变量，它可以被filter方法的第二个参数obj绑定
};

var arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, obj) // [8, 4, 9]  
```
* some()、every()
这两个方法类似“断言”（assert），返回一个布尔值，表示判断数组成员是否符合某种条件。被调用时不会改变数组。
它们接受一个函数作为参数，所有数组成员依次执行该函数。该函数接受三个参数：当前成员、当前位置和整个数组，然后返回一个布尔值。
some方法是只要一个成员的返回值是true，则整个some方法的返回值就是true，否则返回false。
every方法是所有成员的返回值都是true，整个every方法才返回true，否则返回false。
```JavaScript
// 语法
array.some(function(currentValue, index, arr), thisArg)

// 示例
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
// true

// 使用箭头函数测试数组元素的值 更简洁
[2, 5, 8, 1, 4].some(x => x > 10);  // false

// 判断数组元素中是否存在某个值
var fruits = ['apple', 'banana', 'mango', 'guava'];
function checkAvailability(arr, val) {
  return arr.some(function(arrVal) {
    return val === arrVal;
  });
}

checkAvailability(fruits, 'banana'); // true

// 注意，对于空数组，some方法返回false，every方法返回true，回调函数都不会执行。
function isEven(x) { return x % 2 === 0 }

[].some(isEven) // false
[].every(isEven) // true
```
* reduce()、reduceRight() 依次处理数组的每个成员，最终累计为一个值。
它们的差别是，reduce是从左到右处理（从第一个成员到最后一个成员），reduceRight则是从右到左（从最后一个成员到第一个成员），其他完全一样。
```JavaScript
// reduce为数组中的每一个元素依次执行callback函数，不包括数组中被删除或从未被赋值的元素，对于空数组是不会执行回调函数的。接受四个参数：
array.reduce(function(accumulator, currentValue, currentIndex, arr), initialValue)

// 求数组里所有值的和
var sum = [1, 2, 3, 4, 5].reduce(function (acc, cur) {
  console.log(acc, cur);
  return acc + cur;
}, 0)
// 1 2
// 3 3
// 6 4
// 10 5
// 15  最后结果
// 可以写成箭头函数的形式
var total = [ 1, 2, 3, 4, 5 ].reduce(( acc, cur ) => acc + cur, 0);  // 15

// 注意：如果没有提供initialValue，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供initialValue，从索引0开始。
// 如果数组为空且没有提供initialValue，会抛出TypeError 。如果数组仅有一个元素（无论位置如何）并且没有提供initialValue， 或者有提供initialValue但是数组为空，那么此唯一值将被返回并且callback不会被执行。
// 因此，提供初始值通常更安全。


// 下面是一个reduceRight方法的例子。
function subtract(prev, cur) {
  return prev - cur;
}
[3, 2, 1].reduce(subtract)       // 0    reduce方法相当于3减去2再减去1
[3, 2, 1].reduceRight(subtract)  // -4   reduceRight方法相当于1减去2再减去3


// 由于这两个方法会遍历数组，所以实际上还可以用来做一些遍历相关的操作。比如，找出字符长度最长的数组成员。
function findLongest(entries) {
  return entries.reduce(function (longest, entry) {
    return entry.length > longest.length ? entry : longest;
  }, '');
}
findLongest(['aaa', 'bb', 'c']) // "aaa"

// 将二维数组转化为一维
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
    return a.concat(b);
},[]);

// 数组去重
let arr = [1,2,1,2,3,5,4,5,3,4,4];
let result = arr.sort().reduce((init, current)=>{
    if(init.length===0 || init[init.length-1]!==current){
        init.push(current);
    }
    return init;
}, []);
result //[1,2,3,4,5]

// 计算数组中每个元素出现的次数
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];
var countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
countedNames  // { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```
* indexOf()，lastIndexOf()
indexOf方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。
```JavaScript
var a = ['a', 'b', 'c'];
a.indexOf('b') // 1
a.indexOf('y') // -1

// indexOf方法还可以接受第二个参数，表示搜索的开始位置。
['a', 'b', 'c'].indexOf('a', 1) // -1

// lastIndexOf方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。
var a = [2, 5, 9, 2];
a.lastIndexOf(2) // 3
a.lastIndexOf(7) // -1

// 注意，这两个方法不能用来搜索NaN的位置，即它们无法确定数组成员是否包含NaN。这是因为这两个方法内部，使用严格相等运算符（===）进行比较，而NaN是唯一一个不等于自身的值。
[NaN].indexOf(NaN) // -1
[NaN].lastIndexOf(NaN) // -1
```
* 链式使用
上面这些数组方法之中，有不少返回的还是数组，所以可以链式使用。
```JavaScript
var users = [
  {name: 'tom', email: 'tom@example.com'},
  {name: 'peter', email: 'peter@example.com'}
];

users
  .map(function (user) {
    return user.email;
  })
  .filter(function (email) {
    return /^t/.test(email);
  })
  .forEach(function (email) {
    console.log(email);
  });
// "tom@example.com"
```

#### 包装对象
对象是 JavaScript 语言最主要的数据类型，三种原始类型的值——数值、字符串、布尔值——在一定条件下，也会自动转为对象，也就是原始类型的“包装对象”。

* 包装对象
所谓“包装对象”，就是分别与数值、字符串、布尔值相对应的Number、String、Boolean三个原生对象。这三个原生对象可以把原始类型的值变成（包装成）对象。
```JavaScript
// 下面代码中，基于原始类型的值，生成了三个对应的包装对象。
var v1 = new Number(123);    // Number {123}
var v2 = new String('abc');  // String {"abc"}
var v3 = new Boolean(true);  // Boolean {true}

typeof v1 // "object"
typeof v2 // "object"
typeof v3 // "object"

// 包装对象的最大目的，首先是使得 JavaScript 的对象涵盖所有的值，其次使得原始类型的值可以方便地调用某些方法。

// Number、String和Boolean如果不作为构造函数调用（即调用时不加new），常常用于将任意类型的值转为数值、字符串和布尔值。
Number(123) // 123
String('abc') // "abc"
Boolean(true) // true

// 总结一下，这三个对象作为构造函数使用（带有new）时，可以将原始类型的值转为对象；作为普通函数使用时（不带有new），可将任意类型的值，转为原始类型的值。
```
* 实例方法
三种包装对象各自提供了许多实例方法。这里介绍两种它们共同具有、从Object对象继承的方法：valueOf和toString。
```JavaScript
// valueOf方法返回包装对象实例对应的原始类型的值。
new Number(123).valueOf()  // 123
new String('abc').valueOf() // "abc"
new Boolean(true).valueOf() // true

// toString方法返回对应的字符串形式。
new Number(123).valueOf()  // 123
new String('abc').valueOf() // "abc"
new Boolean(true).valueOf() // true

// 对于一些特殊值，如null、undefined、false， Boolean对象前面加不加new，会得到完全相反的结果，必须小心。
if (Boolean(false)) {   // false
  console.log('true');  // 无输出
} 

if (new Boolean(false)) {  // Boolean {false}
  console.log('true');    // true
}
```
* 原始类型与实例对象的自动转换 
原始类型的值，可以自动当作包装对象调用，即调用包装对象的属性和方法。这时，JavaScript 引擎会自动将原始类型的值转为包装对象实例，在使用后立刻销毁实例。
```JavaScript
// 比如，字符串可以调用length属性，返回字符串的长度。
var str = 'abc';
str.length // 3
// 等同于
var strObj = new String(str)
strObj.length // 3
strObj // String {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}
// abc是一个字符串，本身不是对象，不能调用length属性。JavaScript 引擎自动将其转为包装对象，在这个对象上调用length属性。调用结束后，这个临时对象就会被销毁。这就叫原始类型与实例对象的自动转换。

// 自动转换生成的包装对象是只读的，无法修改。所以，字符串无法添加新属性。
var s = 'Hello World';
s.x = 123;
s.x // undefined  为字符串s添加了一个x属性，结果无效，总是返回undefined。

// 另一方面，调用结束后，包装对象实例会自动销毁。这意味着，下一次调用字符串的属性时，实际是调用一个新生成的对象，而不是上一次调用时生成的那个对象，所以取不到赋值在上一个对象的属性。如果要为字符串添加属性，只有在它的原型对象String.prototype上定义。
```
* 自定义方法
除了原生的实例方法，包装对象还可以自定义方法和属性，供原始类型的值直接调用。

```JavaScript
// 比如，我们可以新增一个double方法，使得字符串和数字翻倍。
String.prototype.double = function () {
  return this.valueOf() + this.valueOf();
};

'abc'.double()  // abcabc
// 但是，这种自定义方法和属性的机制，只能定义在包装对象的原型上，如果直接对原始类型的变量添加属性，则无效。
```

#### Number 对象
Number对象是数值对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。
* 包装对象 和 工具函数
```JavaScript
// 作为构造函数时，它用于生成值为数值的对象。
var n = new Number(1);  // Number {1}
// 作为工具函数时，它可以将任何类型的值转为数值。
Number(true) // 1
```
* 静态属性
Number对象拥有以下一些静态属性（即直接定义在Number对象上的属性，而不是定义在实例上的属性）。
```JavaScript
Number.POSITIVE_INFINITY：正的无限，指向Infinity。
Number.NEGATIVE_INFINITY：负的无限，指向-Infinity。
Number.NaN：表示非数值，指向NaN。
Number.MIN_VALUE：表示最小的正数（即最接近0的正数，在64位浮点数体系中为5e-324），相应的，最接近0的负数为-Number.MIN_VALUE。
Number.MAX_SAFE_INTEGER：表示能够精确表示的最大整数，即9007199254740991。
Number.MIN_SAFE_INTEGER：表示能够精确表示的最小整数，即-9007199254740991。
```
* 实例方法
Number对象有4个实例方法，都跟将数值转换成指定格式有关。
```JavaScript
Number.prototype.toString()  将一个数值转为字符串形式。
// toString方法可以接受一个参数，表示输出的进制。如果省略这个参数，默认将数值先转为十进制，再输出字符串。
// toString方法只能将十进制的数，转为其他进制的字符串。如果要将其他进制的数，转回十进制，需要使用parseInt方法。
(10).toString() // "10"
(10).toString(2) // "1010"
(10).toString(8) // "12"
(10).toString(16) // "a"
// 通过方括号运算符也可以调用toString方法。
10['toString'](2) // "1010"


Number.prototype.toFixed()  将一个数转为指定位数的小数，然后返回这个小数对应的字符串
// 参数为小数位数，有效范围为0到20，超出这个范围将抛出 RangeError 错误
(10).toFixed(2) // "10.00"
10.005.toFixed(2) // "10.01"


Number.prototype.toExponential()  将一个数转为科学计数法形式
// 参数是小数点后有效数字的位数，范围为0到20，超出这个范围，会抛出一个 RangeError 错误
(10).toExponential()  // "1e+1"
(10).toExponential(1) // "1.0e+1"


Number.prototype.toPrecision()  将一个数转为指定位数的有效数字
// 参数为有效数字的位数，范围是1到21，超出这个范围会抛出 RangeError 错误
(12.34).toPrecision(1) // "1e+1"
(12.34).toPrecision(2) // "12"
(12.34).toPrecision(3) // "12.3"
// toPrecision方法用于四舍五入时不太可靠，跟浮点数不是精确储存有关。
(12.15).toPrecision(3) // "12.2"
(12.45).toPrecision(3) // "12.4"
```
* 自定义方法
与其他对象一样，Number.prototype对象上面可以自定义方法，被Number的实例继承。
```JavaScript
// 由于add方法返回的还是数值，所以可以链式运算
Number.prototype.subtract = function (x) {
  return this - x;
};
(8).add(2).subtract(4)  // 6

// 注意，数值的自定义方法，只能定义在它的原型对象Number.prototype上面，数值本身是无法自定义属性的。
var n = 1;
n.x = 1;
n.x // undefined
// 上面代码中，n是一个原始类型的数值。直接在它上面新增一个属性x，不会报错，但毫无作用，总是返回undefined。这是因为一旦被调用属性，n就自动转为Number的实例对象，调用结束后，该对象自动销毁。所以，下一次调用n的属性时，实际取到的是另一个对象，属性x当然就读不出来。
```

#### String 对象
String对象是 JavaScript 原生提供的三个包装对象之一，用来生成字符串对象。
```JavaScript
var s1 = 'abc';
var s2 = new String('abc');   // String {"abc"}
typeof s1 // "string"
typeof s2 // "object"
s2.valueOf() // "abc"   返回的就是它所对应的原始字符串

// 字符串对象是一个类似数组的对象（很像数组，但不是数组）。有数值键（0、1、2）和length属性，所以可以像数组那样取值。
new String('abc')  // String {0: "a", 1: "b", 2: "c", length: 3}
(new String('abc'))[1] // "b"

// 除了用作构造函数，String对象还可以当作工具方法使用，将任意类型的值转为字符串。
String(true) // "true"
String(5) // "5"
```
* 静态方法 (即定义在对象本身，而不是定义在对象实例的方法）
String.fromCharCode()  该方法的参数是一个或多个数值，代表 Unicode 码点，返回值是这些码点组成的字符串。
```JavaScript
String.fromCharCode()   // ""    参数为空，就返回空字符串
String.fromCharCode(97) // "a"   否则，返回参数对应的 Unicode 字符串。
String.fromCharCode(104, 101, 108, 108, 111)  // "hello"

// 注意，该方法不支持 Unicode 码点大于0xFFFF的字符，即传入的参数不能大于0xFFFF（即十进制的 65535）。因码点大于0xFFFF的字符占用四个字节，而 JavaScript 默认支持两个字节的字符。这种情况下，必须拆成两个字符表示。
```
* 实例属性
String.prototype.length  字符串实例的length属性返回字符串的长度。 如'abc'.length // 3

* 实例方法
```JavaScript
str.charAt()  返回指定位置的字符，参数是从0开始编号的位置
var s = new String('abc');
s.charAt(1) // "b"
// 这个方法完全可以用数组下标替代。
'abc'[1] // "b"
// 如果参数为负数，或大于等于字符串的长度，charAt返回空字符串。
'abc'.charAt(-1) // ""


str.charCodeAt()  方法返回字符串指定位置的 Unicode 码点（十进制表示），相当于String.fromCharCode()的逆操作
'abc'.charCodeAt(1) // 98
// 如果没有任何参数，charCodeAt返回首字符的 Unicode 码点。如果参数为负数，或大于等于字符串的长度，charCodeAt返回NaN。


str.concat()  用于连接两个字符串，返回一个新字符串，不改变原字符串。
可以接受多个参数。如果参数不是字符串，concat方法会将其先转为字符串，然后再连接。
'a'.concat('b', 'c') // "abc"
// 作为对比，加号运算符在两个运算数都是数值时，不会转换类型
''.concat(1, 2, 3) // "123"
1 + 2 + 3  // 6


str.slice(start,end)  从原字符串取出子字符串并返回，不改变原字符串。它的第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含）
'JavaScript'.slice(0, 4) // "Java"


str.substring(start,stop)从原字符串取出子字符串并返回，不改变原字符串。第一个参数表示子字符串的开始位置，第二个位置表示结束位置（不含）。不建议使用。
'JavaScript'.substring(0, 4) // "Java"
// 如果第一个参数大于第二个参数，substring方法会自动更换两个参数的位置。 如果参数是负数，substring方法会自动将负数转为0。
'JavaScript'.substring(4, -3) // "Java"


str.substr(start,length)  从原字符串取出子字符串并返回，不改变原字符串，跟slice和substring方法的作用相同。第一个参数是子字符串的开始位置（从0开始），第二个参数是子字符串的长度。如果省略第二个参数，则表示子字符串一直到原字符串的结束。
'JavaScript'.substr(4, 6) // "Script"
// 如果第一个参数是负数，表示倒数计算的字符位置。如果第二个参数是负数，将被自动转为0，因此会返回空字符串。
'JavaScript'.substr(-6) // "Script"
'JavaScript'.substr(4, -1) // ""

str.indexOf() indexOf方法用于确定一个字符串在另一个字符串中第一次出现的位置，返回结果是匹配开始的位置。如果返回-1，就表示不匹配。indexOf方法还可以接受第二个参数，表示从该位置开始向后匹配。

str.lastIndexOf()  lastIndexOf方法的用法跟indexOf方法一致，主要的区别是lastIndexOf从尾部开始匹配，indexOf则是从头部开始匹配。第二个参数表示从该位置起向前匹配。


str.trim()  trim方法用于去除字符串两端的空格，返回一个新字符串，不改变原字符串。
// 该方法去除的不仅是空格，还包括制表符（\t、\v）、换行符（\n）和回车符（\r）。
'  hello world  '.trim()   // "hello world"
'\r\nabc \t'.trim() // 'abc'

str.toLowerCase()  将一个字符串全部转为小写。返回一个新字符串，不改变原字符串

str.toUpperCase()  将一个字符串全部转为大写。返回一个新字符串，不改变原字符串


str.match(regexp)  用于确定原字符串是否匹配某个子字符串，返回一个数组，成员为匹配的第一个字符串。如果没有找到匹配，则返回null。
'cat, bat, sat, fat'.match('at') // ["at"]
'cat, bat, sat, fat'.match('xt') // null
// 返回的数组还有index属性和input属性，分别表示匹配字符串开始的位置和原始字符串。
var matches = 'cat, bat, sat, fat'.match('at');
matches.index // 1
matches.input // "cat, bat, sat, fat"


str.search(regexp)  用法基本等同于match，但是返回值为匹配的第一个位置。如果没有找到匹配，则返回-1。
'cat, bat, sat, fat'.search('at') // 1


str.replace(regexp, replacement)  用于替换匹配的子字符串，一般情况下只替换第一个匹配（除非使用带有g修饰符的正则表达式）
'aaa'.replace('a', 'b') // "baa"


str.split(separator, limit)  用指定的分隔符分割成字符串数组，以将字符串分隔为子字符串，以确定每个拆分的位置
'a|b|c'.split('|') // ["a", "b", "c"]
// 如果分割规则为空字符串，则返回数组的成员是原字符串的每一个字符。与 Array.join() 执行的操作相反。
'a|b|c'.split('') // ["a", "|", "b", "|", "c"]   
// 如果省略参数，则返回数组的唯一成员就是原字符串。
'a|b|c'.split() // ["a|b|c"]
// 如果满足分割规则的两个部分紧邻着（即两个分割符中间没有其他字符），则返回数组之中会有一个空字符串。
'a||c'.split('|') // ['a', '', 'c']
// split方法还可以接受第二个参数，限定返回数组的最大成员数。
'a|b|c'.split('|', 2) // ["a", "b"]
```

#### Math 对象
Math是 JavaScript 的原生对象，提供各种数学功能。该对象不是构造函数，不能生成实例，所有的属性和方法都必须在Math对象上调用。
* 静态属性
Math对象的静态属性，提供以下一些数学常数。这些属性都是只读的，不能修改。
```JavaScript
Math.E：常数e。  // 2.718281828459045
Math.LN2：2 的自然对数。 // 0.6931471805599453
Math.LN10：10 的自然对数。 // 2.302585092994046
Math.LOG2E：以 2 为底的e的对数。 // 1.4426950408889634
Math.LOG10E：以 10 为底的e的对数。 // 0.4342944819032518
Math.PI：常数π。 // 3.141592653589793
Math.SQRT1_2：0.5 的平方根。 // 0.7071067811865476
Math.SQRT2：2 的平方根。 // 1.4142135623730951
```
* 静态方法
Math对象提供以下一些静态方法。
```JavaScript
Math.abs()：绝对值

Math.ceil()：向上取整，大于参数值的最小整数（天花板值）
Math.floor()：向下取整，小于参数值的最大整数（地板值）
// 实现一个总是返回数值的整数部分的函数
function ToInteger(x) {
  x = Number(x);
  return x < 0 ? Math.ceil(x) : Math.floor(x);
}

Math.max()：最大值  // 如果参数为空, 返回-Infinity
Math.min()：最小值  // 如果参数为空, 返回Infinity

Math.pow()：指数运算。以第一个参数为底数、第二个参数为幂的指数值
// Math.pow(2, 3) // 8  等同于 2 ** 3

Math.sqrt()：平方根。如果参数是一个负值，则返回NaN
Math.log()：自然对数
Math.exp()：e的指数。返回常数e的参数次方
Math.round()：四舍五入。
// 注意，它对负数的处理（主要是对0.5的处理）。
// Math.round(-1.1) // -1
// Math.round(-1.5) // -1
// Math.round(-1.6) // -2

Math.random()：伪随机数，[0, 1)之间
// 任意范围的随机数生成函数如下。
function getRandomArbitrary(min, max) {
  return Math.random() * (max - min) + min;
}
// 任意范围的随机整数生成函数如下。
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```
* 三角函数方法
```JavaScript
Math.sin()：返回参数的正弦（参数为弧度值）
Math.cos()：返回参数的余弦（参数为弧度值）
Math.tan()：返回参数的正切（参数为弧度值）
Math.asin()：返回参数的反正弦（返回值为弧度值）
Math.acos()：返回参数的反余弦（返回值为弧度值）
Math.atan()：返回参数的反正切（返回值为弧度值
```

#### Date 对象
Date对象是 JavaScript 原生的时间库。它以国际标准时间（UTC）1970年1月1日00:00:00作为时间的零点，可以表示的时间范围是前后各1亿天（单位为毫秒）。
* 普通函数的用法
```JavaScript
// 注意，即使带有参数，Date作为普通函数使用时，返回的还是当前时间。
Date()  // "Mon Mar 04 2019 23:24:09 GMT+0800 (China Standard Time)"
Date(2019, 3, 4)   // 返回结果一样，参数不生效
```
* 构造函数的用法
Date还可以当作构造函数使用。对它使用new命令，会返回一个Date对象的实例。如果不加参数，实例代表的就是当前时间。
```JavaScript
var today = new Date();

// Date实例有一个独特的地方。其他对象求值的时候，都是默认调用.valueOf()方法，但是Date实例求值的时候，默认调用的是toString()方法。
// 这导致对Date实例求值，返回的是一个字符串，代表该实例对应的时间。
var today = new Date();
today // Mon Mar 04 2019 23:27:51 GMT+0800 (China Standard Time)
// 等同于
today.toString() 

// 作为构造函数时，Date对象可以接受多种格式的参数，返回一个该参数对应的时间实例。
// 参数为时间零点开始计算的毫秒数
new Date(1551713492442)  // Mon Mar 04 2019 23:31:32 GMT+0800 (China Standard Time)
// 参数为日期字符串
new Date('Mar 4, 2019');
// 参数为多个整数。 代表年、月、日、小时、分钟、秒、毫秒
new Date(2019, 2, 4, 0, 0, 0, 0)
// 参数可以是负数，表示从基准日扣去相应的时间。
// 只要是能被Date.parse()方法解析的字符串，都可以当作参数。
// 参数为年、月、日等多个整数时，年和月是不能省略的，其他参数都可以省略的。因为如果只使用“年”这一个参数，Date会将其解释为毫秒数。
// 各个参数的取值范围如下，
// 年：使用四位数年份，比如2000。如果写成两位数或个位数，则加上1900，即10代表1910年。如果是负数，表示公元前。
// 月：0表示一月，依次类推，11表示12月。
// 日：1到31。
// 小时：0到23。
// 分钟：0到59。
// 秒：0到59
// 毫秒：0到999
// 注意，月份从0开始计算，但是，天数从1开始计算。另外，除了日期的默认值为1，小时、分钟、秒钟和毫秒的默认值都是0。
// 这些参数如果超出了正常范围，会被自动折算。比如，日期设为0，就代表上个月的最后一天；如果月设为15，就折算为下一年的4月。
```
* 日期的运算
类型自动转换时，Date实例如果转为数值，则等于对应的毫秒数；如果转为字符串，则等于对应的日期字符串。所以，两个日期实例对象进行减法运算时，返回的是它们间隔的毫秒数；进行加法运算时，返回的是两个字符串连接而成的新字符串。
```JavaScript
var d1 = new Date(2019, 2, 4); // 表示2019.3.4
var d2 = new Date(2019, 2, 5); // 表示2019.3.5
d2 - d1  // 86400000
d2 + d1  // "Tue Mar 05 2019 00:00:00 GMT+0800 (China Standard Time)Mon Mar 04 2019 00:00:00 GMT+0800 (China Standard Time)"
```
* 静态方法
```JavaScript
Date.now() 方法返回当前时间距离时间零点（1970年1月1日 00:00:00 UTC）的毫秒数，相当于 Unix 时间戳乘以1000。
Date.now() // 1551747545162


Date.parse() 方法用来解析日期字符串，返回该时间距离时间零点（1970年1月1日 00:00:00）的毫秒数。
// 日期字符串应该符合 RFC 2822 和 ISO 8061 这两个标准，即YYYY-MM-DDTHH:mm:ss.sssZ格式，其中最后的Z表示时区。但是，其他格式也可以被解析
Date.parse('2019-03-05T09:41:00')  // 1551750060000
Date.parse('Mon, 05 Mar 2019 09:41:00 GMT')
// 如果解析失败，返回NaN。
Date.parse('xxx') // NaN


Date.UTC() 方法接受年、月、日等变量作为参数，返回该时间距离时间零点（1970年1月1日 00:00:00 UTC）的毫秒数。
// 该方法的参数用法与Date构造函数完全一致，比如月从0开始计算，日期从1开始计算。
// 区别在于Date.UTC方法的参数，会被解释为 UTC 时间（世界标准时间），Date构造函数的参数会被解释为当前时区的时间。
// 格式
Date.UTC(year, month[, date[, hrs[, min[, sec[, ms]]]]])
// 用法
Date.UTC(2019, 2, 5, 9, 45, 0, 567)  // 1551779100567
```
* 实例方法
Date的实例对象，有几十个自己的方法，除了valueOf和toString，可以分为以下三类。
to类：从Date对象返回一个字符串，表示指定的时间。
get类：获取Date对象的日期和时间。
set类：设置Date对象的日期和时间。
```JavaScript
valueOf() 方法返回实例对象距离时间零点（1970年1月1日00:00:00 UTC）对应的毫秒数，该方法等同于getTime方法。
// 预期为数值的场合，Date实例会自动调用该方法
var d = new Date();
d.valueOf() // 1551750690389
d.getTime() // 1551750690389


toString() 方法返回一个完整的日期字符串。
// 因为toString是默认的调用方法，所以如果直接读取Date实例，就相当于调用这个方法。
var d = new Date(2019, 2, 5);
d.toString()  // "Tue Mar 05 2019 00:00:00 GMT+0800 (China Standard Time)"

toUTCString() 方法返回对应的 UTC 时间，也就是比北京时间晚8个小时。

toISOString() 方法返回对应时间的 ISO8601 写法。UTC 时区的时间。
new Date(2019, 2, 5).toISOString()  // "2019-03-05T02:02:36.471Z"

toJSON()  返回一个符合 JSON 格式的 ISO 日期字符串，与toISOString方法的返回结果完全相同

toDateString()  方法返回日期字符串（不含小时、分和秒）
toTimeString() 方法返回时间字符串（不含年月日）。


toLocaleString([locales[, options]])  完整的本地时间。 locales是一个指定所用语言的字符串，options是一个配置对象
toLocaleDateString()：本地日期（不含小时、分和秒）
toLocaleTimeString()：本地时间（不含年月日）
new Date().toLocaleTimeString('en-US', {
  hour12: false    // "10:12:57"
})  


// 下面这些get*方法返回的都是当前时区的时间，Date对象还提供了这些方法对应的 UTC 版本，用来返回 UTC 时间。 如getUTCDate()
getTime()：返回实例距离1970年1月1日00:00:00的毫秒数，等同于valueOf方法。
getDate()：返回实例对象对应每个月的几号（从1开始）。
getDay()：返回星期几，星期日为0，星期一为1，以此类推。
getFullYear()：返回四位的年份。
getMonth()：返回月份（0表示1月，11表示12月）。
getHours()：返回小时（0-23）。
getMilliseconds()：返回毫秒（0-999）。
getMinutes()：返回分钟（0-59）。
getSeconds()：返回秒（0-59）。


// 这些方法基本是跟get*方法一一对应的，但是没有setDay方法，因为星期几是计算出来的，而不是设置的。另外，需要注意的是，凡是涉及到设置月份，都是从0开始算的，即0是1月，11是12月。
// set*方法的参数都会自动折算。以setDate为例，如果参数超过当月的最大天数，则向下一个月顺延，如果参数是负数，表示从上个月的最后一天开始减去的天数。
// set*系列方法除了setTime()，都有对应的 UTC 版本，即设置 UTC 时区的时间。
setDate(date)：设置实例对象对应的每个月的几号（1-31），返回改变后毫秒时间戳。
setFullYear(year [, month, date])：设置四位年份。
setHours(hour [, min, sec, ms])：设置小时（0-23）。
setMilliseconds()：设置毫秒（0-999）。
setMinutes(min [, sec, ms])：设置分钟（0-59）。
setMonth(month [, date])：设置月份（0-11）。
setSeconds(sec [, ms])：设置秒（0-59）。
setTime(milliseconds)：设置毫秒时间戳

// set类方法和get类方法，可以结合使用，得到相对时间。
var d = new Date();
// 将日期向后推10天
d.setDate(d.getDate() + 10);
```

#### RegExp 对象
RegExp对象提供正则表示式的功能。JavaScript 的正则表达式体系是参照 Perl 5 建立的。
```JavaScript
// 新建正则表达式有两种方法。
var regex = /xyz/i; // 字面量，以斜杠表示开始和结束。在引擎编译代码时，就会新建正则表达式。效率较高，比较便利和直观
// 等价于
var regex = new RegExp('xyz', 'i');  // RegExp构造函数。在运行时新建正则表达式
// 实例属性修饰符相关: ignoreCase; global; multiline
// 实例属性修饰符无关: lastIndex 下一次开始搜索的位置。可读写，只在进行连续搜索时有意义; source返回正则表达式的字符串形式（不包括反斜杠），该属性只读


// 实例方法 RegExp.prototype.test() 和 RegExp.prototype.exec()
test()  返回一个布尔值，表示当前模式是否能匹配参数字符串
/cat/.test('cats and dogs') // true

exec()  返回匹配结果。如果发现匹配，就返回一个数组，成员是匹配成功的子字符串，否则返回null
/x/.exec('_x_x')  // ["x"]


// 字符串的实例方法，有4种与正则表达式有关，String.prototype.match()等。
match()：字符串的match方法与正则对象的exec方法非常类似：匹配成功返回一个数组，匹配失败返回null。如果正则表达式带有g修饰符，则该方法与正则对象的exec方法行为不同，会一次性返回所有匹配成功的结果。
'_x_x'.match(/x/)   // ["x"] 

search()：返回第一个满足条件的匹配结果在整个字符串中的位置。如果没有任何匹配，则返回-1
'_x_x'.search(/x/)  // 1

replace(search, replacement)：替换匹配的值。它接受两个参数，第一个是正则表达式，表示搜索模式，第二个是替换的内容。正则表达式如果不加g修饰符，就替换第一个匹配成功的值，否则替换所有匹配成功的值。
'aaa'.replace('a', 'b') // "baa"
'aaa'.replace(/a/g, 'b') // "bbb"
// replace方法的一个应用，就是消除字符串首尾两端的空格。
var str = '  #id div.class  ';
str.replace(/^\s+|\s+$/g, '')   // "#id div.class"

split(separator, [limit])：按照正则规则分割字符串，返回一个由分割后的各个部分组成的数组。该方法接受两个参数，第一个参数是正则表达式，表示分隔规则，第二个参数是返回数组的最大成员数。
// 正则分隔，去除多余的空格，并指定返回数组的最大成员
'a,  b,c, d'.split(/, */, 2)  [ 'a', 'b' ]


// 预定义模式，某些常见模式的简写方式
\d 匹配0-9之间的任一数字，相当于[0-9]。
\D 匹配所有0-9以外的字符，相当于[^0-9]。
\w 匹配任意的字母、数字和下划线，相当于[A-Za-z0-9_]。
\W 除所有字母、数字和下划线以外的字符，相当于[^A-Za-z0-9_]。
\s 匹配空格（包括换行符、制表符、空格符等），相等于[ \t\r\n\v\f]。
\S 匹配非空格的字符，相当于[^ \t\r\n\v\f]。
\b 匹配词的边界。
\B 匹配非词边界，即在词的内部。
如，/\bworld/.test('helloworld') // false


// 量词符，用来设定某个模式出现的次数。
? 问号，0次或1次，等同于{0, 1}
* 星号，0次或多次，等同于{0,}
+ 加号，1次或多次，等同于{1,}
```

#### JSON 对象
JSON 格式（JavaScript Object Notation）是一种用于数据交换的文本格式，2001年由 Douglas Crockford 提出，目的是取代繁琐笨重的 XML 格式。JSON 格式书写简单，一目了然；符合 JavaScript 原生语法，可以由解释引擎直接处理，不用另外添加解析代码。

JSON 对值的类型和格式有严格的规定。注意，null、空数组和空对象都是合法的 JSON 值。
复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）。
字符串必须使用双引号表示，不能使用单引号。
对象的键名必须放在双引号里面。
数组或对象最后一个成员的后面，不能加逗号。

JSON对象是 JavaScript 的原生对象，用来处理 JSON 格式数据。它有两个静态方法：JSON.stringify()和JSON.parse()。
* JSON.stringify() 方法用于将一个值转为 JSON 字符串。该字符串符合 JSON 格式，并且可以被JSON.parse方法还原。
```JavaScript
// 对于原始类型的字符串，转换结果会带双引号。因为将来还原的时候，内层双引号可以让引擎知道，这是一个字符串，而不是其他类型的值。
JSON.stringify('abc') // ""abc""
JSON.stringify([1, "false", false])  // "[1,"false",false]"
JSON.stringify({ name: "张三" })  // "{"name":"张三"}"
// 如果对象的属性是undefined、函数或 XML 对象，该属性会被JSON.stringify过滤。
var obj = {a: undefined, b: function () {}};
JSON.stringify(obj) // "{}"
// 如果数组的成员是undefined、函数或 XML 对象，则这些值被转成null。
var arr = [undefined, function () {}];
JSON.stringify(arr) // "[null,null]"
// 正则对象会被转成空对象。
JSON.stringify(/foo/) // "{}"
// JSON.stringify方法会忽略对象enumerable为false的属性。
var obj = {};
Object.defineProperties(obj, {
  'bar': {
    value: 2,
    enumerable: false
  }
});
JSON.stringify(obj); // "{}"

// JSON.stringify方法还可以接受一个数组，作为第二个参数，指定需要转成字符串的属性。
var obj = {
  'prop1': 'value1',
  'prop2': 'value2',
  'prop3': 'value3'
};
var selectedProperties = ['prop1', 'prop2'];
JSON.stringify(obj, selectedProperties)  // "{"prop1":"value1","prop2":"value2"}"
// 这个类似白名单的数组，只对对象的属性有效，对数组无效。
JSON.stringify(['a', 'b'], ['0'])  // "["a","b"]"
// 第二个参数还可以是一个函数，用来更改JSON.stringify的返回值。如果处理函数返回undefined或没有返回值，则该属性会被忽略。
function f(key, value) {
  if (typeof value === "number") {
    value = 2 * value;
  }
  return value;
}
JSON.stringify({ a: 1, b: 2 }, f)  // '{"a": 2,"b": 4}'

// 还可以接受第三个参数，用于增加返回的 JSON 字符串的可读性。如果是数字，表示每个属性前面添加的空格（最多不超过10个）；如果是字符串（不超过10个字符），则该字符串会添加在每行前面。
JSON.stringify({ p1: 1, p2: 2 }, null, 2);   // "{  "p1": 1,  "p2": 2  }"


// JSON.stringify发现参数对象有toJSON方法，就直接使用这个方法的返回值作为参数，而忽略原对象的其他参数。
var user = {
  firstName: '三',
  lastName: '张',
  get fullName(){
    return this.lastName + this.firstName;
  },
  toJSON: function () {
    return {
      name: this.lastName + this.firstName
    };
  }
};
JSON.stringify(user)  // "{"name":"张三"}"   若没有 //toJSON方法，则为 "{"firstName":"三","lastName":"张","fullName":"张三"}"
// Date对象就有一个自己的toJSON方法。
var date = new Date('2015-03-05');
date.toJSON() // "2015-03-05T00:00:00.000Z"
JSON.stringify(date) // "2015-03-05T00:00:00.000Z"
// toJSON方法的一个应用是，将正则对象自动转为字符串。因为JSON.stringify默认不能转换正则对象，但是设置了toJSON方法以后，就可以转换正则对象了。
var obj = {reg: /foo/};
JSON.stringify(obj) // "{"reg":{}}"   不设置 toJSON 方法时
RegExp.prototype.toJSON = RegExp.prototype.toString;  
JSON.stringify(/foo/)  // ""/foo/""   设置 toJSON 方法时
```
* JSON.parse()  
```JavaScript
// 方法用于将 JSON 字符串转换成对应的值
JSON.parse('{}') // {}
JSON.parse('{"name": "张三"}');  // {name: "张三"}
// 如果传入的字符串不是有效的 JSON 格式，JSON.parse方法将报错。
JSON.parse("'String'")  // Uncaught SyntaxError: Unexpected token ' in JSON   
// 为了处理解析错误，可以将JSON.parse方法放在try...catch代码块中。
try {
  JSON.parse("'String'");
} catch(e) {
  console.log('parsing error');
}

// JSON.parse方法可以接受一个处理函数，作为第二个参数，用法与JSON.stringify方法类似。如，
function f(key, value) {
  if (key === 'a') {
    return value + 10;
  }
  return value;
}
JSON.parse('{"a": 1, "b": 2}', f)  // {a: 11, b: 2}
```


### 面向对象编程

#### 实例对象与 new 命令
JavaScript 语言具有很强的面向对象编程能力，本章介绍 JavaScript 面向对象编程的基础知识。

##### 对象是什么
面向对象编程（Object Oriented Programming，OOP）是目前主流的编程范式。它将真实世界各种复杂的关系，抽象为一个个对象，然后由对象之间的分工与合作，完成对真实世界的模拟。

对象可以复用，通过继承机制还可以定制。因此，面向对象编程具有灵活、代码可复用、高度模块化等特点，容易维护和开发，比起由一系列函数或指令组成的传统的过程式编程（procedural programming），更适合多人合作的大型软件项目。

对象是单个实物的抽象。当实物被抽象成对象，实物之间的关系就变成了对象之间的关系，从而就可以模拟现实情况，针对对象进行编程。
对象是一个容器，封装了属性（property）和方法（method）。属性是对象的状态，方法是对象的行为（完成某种任务）。

##### 构造函数
面向对象编程的第一步，就是要生成对象。对象是单个实物的抽象。通常需要一个模板，表示某一类实物的共同特征，然后对象根据这个模板生成。

典型的面向对象编程语言（比如 C++ 和 Java），都有“类”（class）这个概念。所谓“类”就是对象的模板，对象就是“类”的实例。但是，JavaScript 语言的对象体系，不是基于“类”的，而是基于构造函数（constructor）和原型链（prototype）。

JavaScript 语言使用构造函数（constructor）作为对象的模板。所谓”构造函数”，就是专门用来生成实例对象的函数。它就是对象的模板，描述实例对象的基本结构。一个构造函数，可以生成多个实例对象，这些实例对象都有相同的结构。
```JavaScript
// 构造函数就是一个普通的函数，但是有自己的特征和用法。
var Vehicle = function () {
  this.price = 10000;  // 构造函数内部的this，代表了新生成的实例对象
};
// 上面代码中，Vehicle就是构造函数。为了与普通函数区别，构造函数名字的第一个字母通常大写。
```
构造函数的特点有两个: 函数体内部使用了this关键字，代表了所要生成的对象实例; 生成对象的时候，必须使用new命令。

##### new 命令
new命令的作用，就是执行构造函数，返回一个实例对象。
```JavaScript
// 使用new命令时，根据需要，构造函数也可以接受参数。
var Vehicle = function (p) {
  this.price = p; 
};
var v = new Vehicle(1000);
v.price // 10000

// 如果忘了使用new命令,构造函数就变成了普通函数，并不会生成实例对象。this这时代表全局对象，将造成一些意想不到的结果。
var v = Vehicle();
v // undefined
price // 1000

// 为了保证构造函数必须与new命令一起使用，一个解决办法是，构造函数内部使用严格模式，即第一行加上use strict。
// 由于严格模式中，函数内部的this不能指向全局对象，默认等于undefined，JavaScript 不允许对undefined添加属性,导致不加new调用会报错
function Fubar(foo, bar){
  'use strict';
  this._foo = foo;
  this._bar = bar;
}

Fubar()  // Uncaught TypeError: Cannot set property '_foo' of undefined at Fubar

// 另一个解决办法，构造函数内部判断是否使用new命令，如果发现没有使用，则直接返回一个实例对象。
function Fubar(foo, bar) {
  if (!(this instanceof Fubar)) {
    return new Fubar(foo, bar);
  }
  this._foo = foo;
  this._bar = bar;
}

Fubar(1, 2)._foo // 1
(new Fubar(1, 2))._foo // 1
```

##### new 命令的原理
使用new命令时，它后面的函数依次执行下面的步骤。创建一个空对象，作为将要返回的对象实例。将这个空对象的原型，指向构造函数的prototype属性。将这个空对象赋值给函数内部的this关键字。开始执行构造函数内部的代码。

也就是说，构造函数内部，this指的是一个新生成的空对象，所有针对this的操作，都会发生在这个空对象上。构造函数之所以叫“构造函数”，就是说这个函数的目的，就是操作一个空对象（即this对象），将其“构造”为需要的样子。
```JavaScript
// 如果构造函数内部有return语句，而且return后面跟着一个对象，new命令会返回return语句指定的对象；否则，就会不管return语句，返回this对象。
var Vehicle = function () {
  this.price = 1000;
  return 1000;
};

(new Vehicle()) === 1000  // false
// 上面代码中，构造函数Vehicle的return语句返回一个数值。这时，new命令就会忽略这个return语句，返回“构造”后的this对象。


// 但是，如果return语句返回的是一个跟this无关的新对象，new命令会返回这个新对象，而不是this对象。这一点需要特别引起注意。
var Vehicle = function (){
  this.price = 1000;
  return { price: 2000 };
};

(new Vehicle()).price  // 2000
// 上面代码中，构造函数Vehicle的return语句，返回的是一个新对象。new命令会返回这个对象，而不是this对象。


// 另一方面，如果对普通函数（内部没有this关键字的函数）使用new命令，则会返回一个空对象。
function getMessage() {
  return 'this is a message';
}

var msg = new getMessage();
msg // getMessage {}
// 上面代码中，getMessage是一个普通函数，返回一个字符串。对它使用new命令，会得到一个空对象。这是因为new命令总是返回一个对象，要么是实例对象，要么是return语句指定的对象。本例中，return语句返回的是字符串，所以new命令就忽略了该语句。


// new命令简化的内部流程，可以用下面的代码表示。
function _new(/* 构造函数 */ constructor, /* 构造函数参数 */ params) {
  // 将 arguments 对象转为数组
  var args = [].slice.call(arguments);
  // 取出构造函数
  var constructor = args.shift();
  // 创建一个空对象，继承构造函数的 prototype 属性
  var context = Object.create(constructor.prototype);
  // 执行构造函数
  var result = constructor.apply(context, args);
  // 如果返回结果是对象，就直接返回，否则返回 context 对象
  return (typeof result === 'object' && result != null) ? result : context;
}

// 实例
var actor = _new(Person, '张三', 28);
```

##### new.target
函数内部可以使用new.target属性。如果当前函数是new命令调用，new.target指向当前函数，否则为undefined。
```JavaScript
function f() {
  console.log(new.target === f);
}

f() // false
new f() // true

// 使用这个属性，可以判断函数调用的时候，是否使用new命令。
function f() {
  if (!new.target) {
    throw new Error('请使用 new 命令调用！');
  }
  // ...
}

f() // Uncaught Error: 请使用 new 命令调用！
```

##### Object.create() 创建实例对象 
构造函数作为模板，可以生成实例对象。但是，有时拿不到构造函数，只能拿到一个现有的对象。我们希望以这个现有的对象作为模板，生成新的实例对象，这时就可以使用Object.create()方法。
```JavaScript
var person1 = {
  name: '张三',
  age: 38,
  greeting: function() {
    console.log('Hi! I\'m ' + this.name + '.');
  }
};

var person2 = Object.create(person1);

person2.name // 张三
person2.greeting() // Hi! I'm 张三.
// 上面代码中，对象person1是person2的模板，后者继承了前者的属性和方法。
```

#### this 关键字
在JavaScript 中，一切皆对象，运行环境也是对象，所以函数都是在某个对象之中运行。

* 核心: this总是返回一个对象，this就是属性或方法“当前”所在的对象，即函数运行时所在的对象（环境）。

* 实质
JavaScript 语言之所以有 this 的设计，跟内存里面的数据结构有关系。
```JavaScript
var obj = { foo:  5 };

// 上面的代码将一个对象赋值给变量obj。JavaScript 引擎会先在内存里面，生成一个对象{ foo: 5 }，然后把这个对象的内存地址赋值给变量obj。也就是说，变量obj是一个地址（reference）。后面如果要读取obj.foo，引擎先从obj拿到内存地址，然后再从该地址读出原始的对象，返回它的foo属性。

// 原始的对象以字典结构保存，每一个属性名都对应一个属性描述对象。举例来说，上面例子的foo属性，实际上是以下面的形式保存的。foo属性的值保存在属性描述对象的value属性里面。
{
  foo: {
    [[value]]: 5
    [[writable]]: true
    [[enumerable]]: true
    [[configurable]]: true
  }
}

// 但是，属性的值也可能是一个函数。
var obj = { foo: function () {} };
// 这时，引擎会将函数单独保存在内存中，然后再将函数的地址赋值给foo属性的value属性
{
  foo: {
    [[value]]: 函数的地址
    ...
  }
}

// 由于函数是一个单独的值，函数可以在不同的运行环境（上下文）执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境（context）。所以，this就出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境。
var x = 1;
var obj = { f: f, x: 2 };
var f = function () { console.log(this.x); }

f() // 1  单独执行 
obj.f() // 2   obj 环境执行
```
* 使用场合
this主要有以下几个使用场合。全局环境、构造函数、对象的方法
```JavaScript
// 全局环境。全局环境使用this，它指的就是顶层对象window。
this === window // true
function f() {
  console.log(this === window);
}
f() // true
// 上面代码说明，不管是不是在函数内部，只要是在全局环境下运行，this就是指顶层对象window。


// 构造函数。构造函数中的this，指的是实例对象。
var Obj = function (p) {
  this.p = p;
};
var o = new Obj('Hello World!');
o.p // "Hello World!"
// 上面代码定义了一个构造函数Obj。由于this指向实例对象，所以在构造函数内部定义this.p，就相当于定义实例对象有一个p属性。


// 对象的方法。如果对象的方法里面包含this，this的指向就是方法运行时所在的对象。该方法赋值给另一个对象，就会改变this的指向。
// 下面代码中，obj.foo方法执行时，它内部的this指向obj。
var obj ={
  foo: function () {
    console.log(this);
  }
};
obj.foo() // obj

// 但是，下面这几种用法，都会改变this的指向。obj.foo就是一个值。这个值真正调用的时候，运行环境已经不是obj了，而是全局环境，所以this不再指向obj。
// 情况一
(obj.foo = obj.foo)() // window
// 情况二
(false || obj.foo)() // window
// 情况三
(1, obj.foo)() // window
// 可以这样理解，JavaScript 引擎内部，obj和obj.foo储存在两个内存地址，称为地址一和地址二。obj.foo()这样调用时，是从地址一调用地址二，因此地址二的运行环境是地址一，this指向obj。但是，上面三种情况，都是直接取出地址二进行调用，这样的话，运行环境就是全局环境，因此this指向全局环境。

// 如果this所在的方法不在对象的第一层，这时this只是指向当前一层的对象，而不会继承更上面的层。
var a = {
  p: 'Hello',
  b: {
    m: function() {
      console.log(this.p);
    }
  }
};
a.b.m() // undefined
// 上面代码中，a.b.m方法在a对象的第二层，该方法内部的this不是指向a，而是指向a.b，因为实际执行的是下面的代码。
var b = {
  m: function() {
   console.log(this.p);
  }
};
var a = {
  p: 'Hello',
  b: b
};
(a.b).m() // 等同于 b.m()
// 如果要达到预期效果，只有写成下面这样。
var a = {
  b: {
    m: function() {
      console.log(this.p);
    },
    p: 'Hello'
  }
};

// 如果这时将嵌套对象内部的方法赋值给一个变量，this依然会指向全局对象。
var a = {
  b: {
    m: function() {
      console.log(this.p);
    },
    p: 'Hello'
  }
};
var hello = a.b.m;
hello() // undefined
// 上面代码中，m是多层对象内部的一个方法。为求简便，将其赋值给hello变量，结果调用时，this指向了顶层对象。为了避免这个问题，可以只将m所在的对象赋值给hello，这样调用时，this的指向就不会变。
var hello = a.b;
hello.m() // Hello
```
* 使用注意点
```JavaScript
// 避免多层 this。由于this的指向是不确定的，所以切勿在函数中包含多层的this。
var o = {
  f1: function () {
    console.log(this);  // Object
    var f2 = function () {
      console.log(this);  // Window
    }();
  }
}
o.f1()
// 上面代码包含两层this，结果运行后，第一层指向对象o，第二层指向全局对象，因为实际执行的是下面的代码。
var temp = function () {
  console.log(this);
};
var o = {
  f1: function () {
    console.log(this);
    var f2 = temp();
  }
}
// 一个解决方法是在第二层改用一个指向外层this的变量。
var o = {
  f1: function() {
    console.log(this); // Object
    var that = this;
    var f2 = function() {
      console.log(that);  // Object
    }();
  }
}
// 上面代码定义了变量that，固定指向外层的this，然后在内层使用that，就不会发生this指向的改变。

// JavaScript 提供了严格模式，也可以硬性避免这种问题。严格模式下，如果函数内部的this指向顶层对象，就会报错。
var counter = {
  count: 0
};
counter.inc = function () {
  'use strict';
  this.count++
};
var f = counter.inc;
f()  // Uncaught TypeError: Cannot read property 'count' of undefined at counter.inc
// 上面代码中，inc方法通过'use strict'声明采用严格模式，这时内部的this一旦指向顶层对象，就会报错。


// 此外，还要避免数组处理方法中的 this。数组的map和foreach方法，允许提供一个函数作为参数。这个函数内部不应该使用this。避免回调函数中的 this。
```
* 绑定 this 的方法
```JavaScript
// JavaScript 提供了call、apply、bind这三个方法，来切换/固定this的指向。
Function.prototype.call(thisValue, arg1, arg2, ...)

var obj = {};
var f = function () {
  return this;
};
f() === window // true
f.call(obj) === obj // true
// 上面代码中，全局环境运行函数f时，this指向全局环境（浏览器为window对象）；call方法可以改变this的指向，指定this指向对象obj，然后在对象obj的作用域中运行函数f。
// call方法的参数，应该是一个对象。如果参数为空、null和undefined，则默认传入全局对象。
// 如果call方法的参数是一个原始值，那么这个原始值会自动转成对应的包装对象，然后传入call方法。
var f = function () {
  return this;
};
f.call(5)  // Number {[[PrimitiveValue]]: 5}


Function.prototype.apply(thisValue, [arg1, arg2, ...])
// apply方法的作用与call方法类似，也是改变this指向，然后再调用该函数。唯一的区别就是，它接收一个数组作为函数执行时的参数。

function f(x, y){
  console.log(x + y);
}
f.call(null, 1, 1) // 2
f.apply(null, [1, 1]) // 2
// 上面代码中，f函数本来接受两个参数，使用apply方法以后，就变成可以接受一个数组作为参数。

// 利用这一点，可以做一些有趣的应用。
// 找出数组最大元素。JavaScript 不提供找出数组最大元素的函数。结合使用apply方法和Math.max方法，就可以返回数组的最大元素。
var a = [10, 2, 4, 15, 9];
Math.max.apply(null, a) // 15
// 将数组的空元素变为undefined
Array.apply(null, ['a', ,'b']) // [ 'a', undefined, 'b' ]  数组的forEach方法会跳过空元素，但是不会跳过undefined。
// 转换类似数组的对象。利用数组对象的slice方法，可以将一个类似数组的对象（比如arguments对象）转为真正的数组。
Array.prototype.slice.apply({0: 1, length: 2}) // [1, undefined]   生效前提是被处理的对象必须有length属性，以及相对应的数字键。


Function.prototype.bind()
// bind方法用于将函数体内的this绑定到某个对象，然后返回一个新函数。
var d = new Date();
d.getTime() // 1551934555040
var print = d.getTime;
print() // Uncaught TypeError: this is not a Date object.
// 上面代码中，我们将d.getTime方法赋给变量print，然后调用print就报错了。这是因为getTime方法内部的this，绑定Date对象的实例，赋给变量print以后，内部的this已经不指向Date对象的实例了。
// bind方法可以解决这个问题。
var print = d.getTime.bind(d);  // bind方法将getTime方法内部的this绑定到d对象，这时就可以安全地将这个方法赋值给其他变量了。
print() // 1551934555040   

// bind方法的参数就是所要绑定this的对象，下面是一个更清晰的例子。
var counter = {
  count: 0,
  inc: function () {
    this.count++;
  }
};
var func = counter.inc.bind(counter);  // counter.inc方法被赋值给变量func。这时必须用bind方法将inc内部的this，绑定到counter，否则就会出错。
func();
counter.count // 1
// this绑定到其他对象也是可以的。
var counter = {
  count: 0,
  inc: function () {
    this.count++;
  }
};
var obj = {
  count: 100
};
var func = counter.inc.bind(obj);  // bind方法将inc方法内部的this，绑定到obj对象。结果调用func函数以后，递增的就是obj内部的count属性。
func();
obj.count // 101  
// bind还可以接受更多的参数，将这些参数绑定原函数的参数。
var add = function (x, y) {
  return x * this.m + y * this.n;
}
var obj = { m: 2, n: 2 };
var newAdd = add.bind(obj, 5);  //bind方法除绑定this对象，还将add函数的第一个参数x绑定成5，返回一个新函数newAdd，新函数只要再接受一个参数y就能运行了
newAdd(5) // 20
// 如果bind方法的第一个参数是null或undefined，等于将this绑定到全局对象，函数运行时this指向顶层对象（浏览器为window）。
function add(x, y) {
  return x + y;
}
var plus2 = add.bind(null, 2);
plus2(8) // 10
// 上面代码中，函数add内部并没有this，使用bind方法的主要目的是绑定参数x，以后每次运行新函数plus5，就只需要提供另一个参数y就够了。而且因为add内部没有this，所以bind的第一个参数是null，不过这里如果是其他对象，也没有影响。

// 某些数组方法可以接受一个函数当作参数。这些函数内部的this指向，很可能会出错。
var obj = {
  name: '张三',
  times: [1, 2, 3],
  print: function () {
    this.times.forEach(function (n) {
      console.log(this.name);
    }.bind(this));
  }
};
obj.print()  // 张三 张三 张三
// 上面代码中，obj.print内部this.times的this是指向obj的，这个没有问题。但是，forEach方法的回调函数内部的this.name却是指向全局对象，导致没有办法取到值。通过bind方法绑定this可以解决这个问题。
```
