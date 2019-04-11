---
title: 面试题 HTML和CSS
comments: true
date: 2019-04-10 09:00:00
categories: Review
tags: ['Review', 'HTML', 'CSS']
---

1. 什么是盒子模型
在网页中，把HTML中的元素看做是一个矩形的盒子（盛装内容的的容器），每个容器都是由元素内容content、内边距（padding）、边框（border）和外边距（margin）组成。4个部分一起构成了css中元素的盒模型。

两种盒子模型
w3c的盒子模型：padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 (Element width = width + border + padding ) ，此属性表现为标准模式下的盒模型。 box-sizing:content-box;

IE的盒子模型：padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 (Element width = width ) 。 box-sizing:border-box;

W3C盒模型与IE盒模型的区别就是对宽高的定义不同。W3C认为，宽高是内容区的宽度（只包含节点显示的具体内容）。IE认为，宽高是显示效果的实际效果（包含节点的全部内容）

2. 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？
行内元素：a、b、span、img、input、strong、select、label、em、button、textarea
块级元素：div、ul、li、dl、dt、dd、p、h1-h6、blockquote
空元素：即系没有内容的HTML元素，例如：br、meta、hr、link、input、img

3. href与src的区别
src，source的简写，表示“源”，是引用一个资源，用来整体替换自身的内容。在 img、script、iframe 等元素上使用。
href，Hypertext Reference的缩写，表示“超链接”，可叠加，目的不是为了引用一个资源，而是为了建立联系，让当前标签能够链接到目标地址上。在 link和a 等元素上使用。

4. link与@import之间的区别
两者都是外部引用CSS的方式，但是存在一定的区别：
区别1：link除了引用样式文件，还可以引用图片等资源文件，而import只引用CSS样式文件
区别2：link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
区别3：link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
区别4：ink支持使用Javascript控制DOM去改变样式；而@import不支持。

5. 浏览器的内核(Rendering Engine渲染引擎)
IE: Trident
Firefox：Gecko
Safari：Webkit内核
Opera：以前是Presto内核，现改用Blink内核
Chrome：Blink(基于webkit，Google与Opera Software共同开发)

6. 一次完整的HTTP事务是怎样的一个过程？
基本流程：
a. 域名解析(浏览器自身的DNS缓存->操作系统->hosts文件->本地首选DNS服务器发起域名解析请求)

b. 发起TCP的3次握手(防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。解决网络中存在延迟的重复分组的问题，同时防止服务器端的一直等待而浪费资源)

第一次握手：客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；
第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；
第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

c. 建立TCP连接后发起http请求

d. 服务器端响应http请求，浏览器得到html代码

e. 浏览器解析html代码，并请求html代码中的资源

f. 浏览器对页面进行渲染呈现给用户
![TCP三次握手](/images/tcp-shakehands.jpeg)

7. 常见Web攻击技术
（1）XSS(Cross-Site Scripting 跨站脚本攻击)。攻击者在网页中嵌入恶意脚本程序，使之在用户的浏览器上运行非法的HTML标签或者JavaScript。利用这些恶意脚本，攻击者可获取用户的敏感信息如 Cookie、SessionID 等，进而危害数据安全。根据存储区(恶意代码存放的位置)和 插入点(由谁取得恶意代码，并插入到网页上)分成三类。
反射性XSS: 非持久型，攻击者构造出特殊的URL，其中包含恶意代码，用户打开带有恶意代码的 URL 时，网站服务端将恶意代码从 URL 中取出，拼接在 HTML 中返回给浏览器。由于需要用户主动打开恶意的URL才能生效，攻击者往往会结合多种手段诱导用户点击。常见于通过URL传递参数的功能，如在搜索和跳转。

存储型XSS: 持久化型，攻击者将恶意代码提交到目标网站的数据库中，用户打开目标网站时，网站服务端将恶意代码从数据库取出，拼接在HTML中返回给浏览器，用户浏览器接收到响应后解析执行，混在其中的恶意代码也被执行。常见于带有用户保存数据的网站功能，如论坛、博客、留言板、网站的留言、评论、日志等交互处。

DOM型XSS: 前端JS代码本身不够严谨，把不可信的数据当作代码执行了。完全在前端浏览器触发，无需服务端的参与，这是前端开发工程师的地盘。
特别注意以下输入源进行转移document.URL、location.hash、location.research、document.referrer(此处应尤为注意，referrer属性虽然可用于避免CSRF，但可触发XSS攻击)、XHR返回值（跨域返回值）、form表单及各种input框。
在使用 .innerHTML、.outerHTML、document.write() 时要特别小心，不要把不可信的数据作为HTML插到页面上，应尽量使用 .textContent、.setAttribute() 等。
DOM 中的内联事件监听器，如 location、onclick、onerror、onload、onmouseover 等，a 标签的 href 属性，JavaScript 的 eval()、setTimeout()、setInterval() 等，都能把字符串作为代码运行。如果不可信的数据拼接到字符串中传递给这些 API，很容易产生安全隐患，务必避免。

解决方案：不用DOM中的内联事件监听器；将输入的数据进行转义处理；使用成熟的 Vue/React框架；设置 HTTP Header："X-XSS-Protection: 1"； HTTP-only Cookie

（2）SQL注入攻击。
含义：通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令，比如SELECT * FROM users WHERE 'username' = 'admin' and 'password' = '' OR 1=1' ，这句 sql 无论 username 和 password 是什么都会执行，从而将所有用户的信息取出来。
解决方案：特殊字符过滤，orm框架可以对参数进行转义；对sql语句进行预编译；关闭错误信息；客户端对数据进行加密；控制数据库的权限，比如只能select，不能insert。

（3）CSRF(Cross-Site Request Forgeries，跨站点请求伪造)。
含义：借助本地cookie，伪装成受信任用户的名义发送恶意请求。
解决方案：通过referer识别；采用token认证。

（4）DDos攻击(Distributed Denial of Service Attack 分布式拒绝服务攻击)。
含义：在短时间内发起大量请求，耗尽服务器的资源，无法响应正常的访问，造成网站实质下线。 
攻击方式：Ping Flood攻击即利用ping命令不停的发送的数据包到服务器； SYN Flood攻击即利用tcp协议原理，伪造受害者的ip地址，一直保持与服务器的连接，导致受害者连接服务器的时候拒绝服务。
解决方案：备份网站，全静态网页，以备临时应对；请求过滤拦截(高防服务器、设置防火墙、黑名单)；带宽扩容；检测技术和清洗技术；CDN 加速。

（5）Cookies攻击。
含义：通过document.cookie,立刻就可以看到当前站点的cookie。
解决方案：在cookie打上HttpOnly的标记，这样就无法通过JS来取得。

（6）重定向攻击。
含义：钓鱼攻击者，通常会发送给受害者一个合法链接，当链接被点击时，用户被导向一个似是而非的非法网站，从而达到骗取用户信任、窃取用户资料的目的。
解决方案：白名单,将合法的要重定向的url加到白名单中,非白名单上的域名重定向时拒之；重定向token,在合法的url上加上token,重定向时进行验证。

（7）文件上传攻击。文件名攻击、文件后缀攻击、文件内容攻击。


8. 什么是BFC
BFC（Block Formatting Context）块级格式化上下文。具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。
* 怎样才能形成BFC
(1)body 根元素
(2)display 为 inline-block、table-cells、flex
(3)浮动元素：float 除 none 以外的值
(4)绝对定位元素：position 为absolute、fixed (不为relative和static)
(5)overflow 除了 visible 以外的值 (hidden、auto、scroll)

* BFC布局规则
(1)内部的Box会在垂直方向上一个接一个的放置
(2)垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
(3)每个元素的左外边缘（margin-left)，与包含块的左边相接触(对于从左往右的格式化，否则相反)，即使存在浮动也是如此。除非这个元素自己形成了一个新的BFC。
(4)BFC的区域不会与float的元素区域重叠。
(5)计算BFC的高度时，浮动子元素也参与计算。
(6)BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素，反之亦然。

* 应用
(1)清除浮动。如子元素是float 浮动的，受浮动影响，父元素的高度塌陷。在父元素加上 overflow: hidden 使其形成BFC，则因计算高度时会计算float的子元素的高度，而达到清除浮动影响的效果。
(2)两列自适应布局。
```HTML
<div>
  <div class="left">left</div>
  <div class="right">blablabla...</div>
</div>

.left {
  float: left;  // 会导致right元素的一部分跑到了左侧元素下方
  width: 100px;
  height: 50px;
  background-color: yellow;
}
.right {
  background-color: pink;
  // 触发right元素的BFC。因BFC的区域是独立的，不会与页面其他元素相互影响，且不会与float元素重叠，因此就可以形成两列自适应布局
  overflow: hidden;
}
```
(3)防止垂直margin合并。
```HTML
<div class="a">aaa</div>
<!-- <div class="cap"> -->
  <div class="b">bbbbbbbbbbbbbbbbbbbb</div>
<!-- </div> -->

.left{
  width: 100px;
  height: 100px;
  background: red;
  margin-bottom: 100px;
}
.right{
  height: 100px;
  background: yellow;
  margin-top: 100px;
}

<!-- b元素外增加包裹一层父元素，设置 overflow: hidden 使其形成BFC -->
.cap{
  overflow: hidden; // 因为BFC内部是一个独立的容器，所以不会与外部相互影响，可以防止margin合并
}
```

9. 清除浮动的几种方式
```HTML
<div class="float-div">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
<div class="clear-both"></div>
<div class="normal-div">A</div>

<!-- 对父级设置高度 -->
父级div手动定义height，就解决了父级div无法自动获取到高度的问题。只适合高度固定的布局，要给出精确的高度，如果高度和父级div不一样时，会产生问题。

<!-- 父级标签overflow:hidden -->
.float-div{
  overflow:hidden;
}

<!-- 父级标签定义伪元素::after -->
.float-div::after{
  display: block;
  content: '';
  height: 0px;
  clear: both;
}

<!-- 在浮动元素的最后加一个标签，添加空标签，让父级div能自动获取到高度 clear:both -->
.clear-both{
  clear: both;
}
```
10. flex 设为 1、auto、 none 的区别
flex 是 flex-grow、flex-shrink、flex-basis 三个属性值的组合缩写。假设以上三个属性同样取默认值，则 flex 的默认值是 0 1 auto。

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
flex-grow: number; /* default 0 */

flex-shrink属性定义了项目的收缩比例，默认为1，即如果空间不足，该项目将缩小。如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
flex-shrink: number; /* default 1 */

flex-basis属性定义了在分配多余空间之前，相对于父弹性盒容器的 项目占据的主轴空间  宽或者高（取决于主轴方向）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。
flex-basis: length | auto; /* default auto */

当flex 取默认值，则 flex 的默认值是 0 1 auto。不放大，空间不足则缩小，保持本来大小。

当 flex 取值为 auto，则计算值为 1 1 auto。

当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%，即取 num, 1, 1, 0%
```CSS
.item { flex: 1; }
/* 等同于 */
.item {
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0%;
}
```

当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1，即取 1, 1, num px / num%

当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%

当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1

当 flex 取值为 none，则计算值为 0 0 auto

11. 请写出三栏布局，其中左栏、右栏宽度各为200px，中间自适应
```HTML
<div class="wrap">
  <div class="left">left</div>
  <div class="middle">middle</div>
  <div class="right">right</div>
</div>
```

```CSS
* {
  padding: 0;
  margin: 0;
}
.left, .right, .middle{
  min-height: 200px;
}
/* float  middle要放最后 */
.left{
  float: left;

  width: 200px;
  background: red;
}
.right{
  float: right;

  width: 200px;
  background: yellow;
}
.middle{
  background: blueviolet;
}

/* position   middle要放最后 */
.left{
  position: absolute;
  left: 0;

  width: 200px;
  background: red;
}
.right{
  position: absolute;
  right: 0;

  width: 200px;
  background: yellow;
}
.middle{
  position: absolute;
  left: 200px;
  right: 200px;

  background: blueviolet;
}

/* flex  增加wrap元素，middle放中间 */
.wrap{
  display: flex;
  /* flex-direction: row; */
  /* justify-content: space-between; */
}
.left{
  width: 200px;
  background: red;
}
.right{
  width: 200px;
  background: yellow;
}
.middle{
  flex: 1;

  background: blueviolet;
}

/* 表格布局 table */
.wrap{
  display: table;
  width: 100%;
  heigt: 200px;
}
.left, .right, .middle{
  display: table-cell;
}
.left{
  width: 200px;
  background: red;
}
.right{
  width: 200px;
  background: yellow;
}
.middle{
  background: blueviolet;
}

/* 网格布局 grid */
.wrap{
  display: grid;
  width: 100%;
  grid-template-rows: 200px;
  grid-template-columns: 200px auto 200px;
}
.left{
  background: red;
}
.right{
  background: yellow;
}
.middle{
  background: blueviolet;
}
```

11. ie 低版本兼容解决方案
* 强制使用高版本渲染模式
仍然会有部分低版本用户无法正常渲染。治标不治本。
```HTML
强制使用Edge模式来解析网页代码
<meta http-equiv="X-UA-Compatible" content="IE=Edge">

强制使用Chrome浏览器内核(如果安装了)
<meta http-equiv=”X-UA-Compatible” content=”IE=edge,chrome=1″/>
```
* css hack针对不同浏览器编不同的代码
能够解决一些问题，不能使用浏览器的新特性。一般适用于不太复杂的页面。
```HTML
条件注释，很常用。
<!--[if IE 6]>coding...<![endif]--> //ie6
<!--[if lte IE 9]>coding...<![endif]--> //小于等于ie9

属性前缀
_background-color:#000; //ie6
*background-color:#000; //ie6 ie7
```
* js/css兼容插件
在利用下面hack方式下，针对低版本浏览器加载相应插件。能解决大部分的兼容性问题。需要额外加载JS/CSS文件。
Html5shiv：使浏览器兼容HTML5标签语法。
respond：A fast & lightweight polyfill for min/max-width CSS3 Media Queries (for IE 6-8, and more)
bsie：在IE6上支持大部分bootstrap的特性。
IE-CSS3/PIE：使ie6-9支持CSS3。

* 终极方案：低版本提示升级
判断浏览器版本，低版本的浏览器提示升级，或强制跳转到升级页面。

为什么要强制提示升级的理由：
低版本浏览器份额已经很小，逐渐减小；微软彻底放弃了ie品牌，力推Edge；为了兼容低版本的浏览器浪费大量的时间和精力；html5的新特性无法使用
```JavaScript
// 放在页面中或js文件中 都可以。  直接跳转
(function(w){if(!("WebSocket"in w&&2===w.WebSocket.CLOSING)){w.location.replace("http://browsehappy.osfipin.com/");}}(window));
```