---
title: Web前端性能优化 - 让你的页面飞起来
comments: true
date: 2019-06-24 20:30:00
categories: 性能优化
tags: ['web', '性能优化']
---

> 从构建、浏览器渲染、缓存、PWA、服务端优化等多方面，梳理前端性能优化的技术点、综合分析技术的原理，根据不同的业务场景选择合适的性能优化点进行应用，最终为你的网站带来显著的速度提升和整体性能提升。


资源合并压缩、图片格式及加载方式的优化
CSS和JS的装载与执行
重绘与回流
浏览器存储
PWA，缓存
SSR服务端渲染
构建层减少资源请求的大小和数量
优化浏览器渲染提升动画体验
显著提升网站二次访问体验
首屏渲染性能优化
![总览](/images/performance-optimization.webp)

## 基础
web 前端本质上是一种 GUI 软件，采用 BS 架构，经历开发到发布到 CDN 或 webserver , 通过 浏览器 http 请求返回资源。

### 资源合并与压缩
关键: 减少 http 请求数量、减少请求资源大小

#### http 清求的过程
深入理解 http  请求过程，是 前端性能优化的核心。

用户在浏览器输入 url -> DNS解析(浏览器未过期缓存、操作系统、hosts文件、首选DNS服务器等) -> 与服务器建立连接，发起TCP的3次握手 --> 发起http请求 --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，并请求html代码中的资源 --> 浏览器对页面进行渲染呈现给用户

##### http 是什么
通俗来讲，他就是计算机通过网络进行通信的规则，是一个基于请求与响应，无状态的，应用层的协议，常基于TCP/IP协议传输数据。目前任何终端（手机，笔记本电脑。。）之间进行任何一种通信都必须按照Http协议进行，否则无法连接。

##### 三次握手
![TCP三次握手](/images/tcp-shakehands.jpeg)
客户端的请求到达服务器，首先就是建立TCP连接。

第一次握手：客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；
第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；
第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

##### 为什么要三次握手
防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。解决网络中存在延迟的重复分组的问题，同时防止服务器端的一直等待而浪费资源

##### TCP四次挥手
当客户端和服务器通过三次握手建立了TCP连接以后，当数据传送完毕，肯定是要断开TCP连接的啊。那对于TCP的断开连接，这里就有了神秘的“四次分手”。

第一次分手：主机1（可以使客户端，也可以是服务器端），设置Sequence Number，向主机2发送一个FIN报文段；此时，主机1进入FIN_WAIT_1状态；这表示主机1没有数据要发送给主机2了；
第二次分手：主机2收到了主机1发送的FIN报文段，向主机1回一个ACK报文段，Acknowledgment Number为Sequence Number加1；主机1进入FIN_WAIT_2状态；主机2告诉主机1，我“同意”你的关闭请求；
第三次分手：主机2向主机1发送FIN报文段，请求关闭连接，同时主机2进入LAST_ACK状态；
第四次分手：主机1收到主机2发送的FIN报文段，向主机2发送ACK报文段，然后主机1进入TIME_WAIT状态；主机2收到主机1的ACK报文段以后，就关闭连接；此时，主机1等待2MSL后依然没有收到回复，则证明Server端已正常关闭，那好，主机1也可以关闭连接了。

##### 为什么要四次分手
TCP协议是一种面向连接的、可靠的、基于字节流的运输层通信协议。TCP是全双工模式，这就意味着，当主机1发出FIN报文段时，只是表示主机1已经没有数据要发送了，主机1告诉主机2，它的数据已经全部发送完毕了；但是，这个时候主机1还是可以接受来自主机2的数据；当主机2返回ACK报文段时，表示它已经知道主机1没有数据发送了，但是主机2还是可以发送数据到主机1的；当主机2也发送了FIN报文段时，这个时候就表示主机2也没有数据要发送了，就会告诉主机1，我也没有数据要发送了，之后彼此就会愉快的中断这次TCP连接。

##### 为什么HTTP协议要基于TCP来实现
TCP是一个端到端的可靠的面向连接的协议，所以HTTP基于传输层TCP协议不用担心数据的传输的各种问题。

#### 具体优化方法
#####  内容优化
(1)减少HTTP请求数:这条策略是最重要最有效的
(2)减少DNS查找
(3)避免重定向
(4)使用Ajax缓存
(5)延迟加载组件,预加载组件
(6)减少DOM元素数量:页面中存在大量DOM元素,会导致javascript遍历DOM的效率变慢。
(7)最小化iframe的数量：iframes 提供了一个简单的方式把一个网站的内容嵌入到另一个网站中。但其创建速度比其他包括js和css的DOM元素的创建慢了1-2个数量级。
(8)避免404：HTTP请求时间消耗是很大的，因此使用HTTP请求来获得一个没有用处的响应（例如404没有找到页面）是完全没有必要的，它只会降低用户体验而不会有一点好处。

##### html压缩
压缩在文本文件中有意义，但在 html 中不显示的字符，如 空格、制表符、换行符，注释。

可以使用在线网站、nodejs提供的 html-minifier、后端模板引擎渲染压缩

##### css 优化
无效代码删除, CSS 代码语义合并
(1)将CSS代码放在HTML页面的顶部
(2)避免使用CSS表达式
(3)使用 link 来代替@import
(4)避免使用Filters

##### js 优化
(1)将JavaScript脚本放在页面的底部。
(2)将JavaScript和CSS作为外部文件来引用：在实际应用中使用外部文件可以提高页面速度，因为JavaScript和CSS文件都能在浏览器中产生缓存。
(3)缩小JavaScript，无效字符删除、剔除注释、代码语义合并，可以使用 uglifyjs 压缩。
(4)删除重复的脚本
(5)最小化DOM的访问：使用JavaScript访问DOM元素比较慢。
(6)开发智能的事件处理程序
(7)javascript代码注意：谨慎使用with,避免使用eval Function函数,减少作用域链查找。

##### Cookie优化
(1)减小Cookie大小
(2)针对Web组件使用域名无关的Cookie

##### 服务器优化
(1)使用内容分发网络（CDN）：把网站内容分散到多个、处于不同地域位置的服务器上可以加快下载速度。
(2)GZIP压缩
(3)设置ETag：ETags（Entity tags，实体标签）是web服务器和浏览器用于判断浏览器缓存中的内容和服务器中的原始内容是否匹配的一种机制。
(4)提前刷新缓冲区
(5)对Ajax请求使用GET方法
(6)避免空的图像src

##### 文件合并
使用 keep-alive，公共库合并、不同页面的合并。减少丢包和网络请求延迟。但要适当使用，避免 首屏渲染问题

##### 开启 gzip

##### 构建层面
在线网站或结合 webpack、fis3 等构建工具，自动化地进行资源的打包和压缩

##### 图片相关的优化
雪碧图（csssprits），将多个小图整合成一张大图

不同格式图片常用的业务场景
1、JPEG(Joint Photographic Experts Group) 有损压缩，压缩率高，占内存小，不支持透明
主要用于摄影作品或者写实作品（或是其他细节、色彩丰富的图片）或大的背景图；对多色彩表现较好；不适于文字较多的图片。在页面中使用的商品图片、采用人像或者实物素材制作的广告Banner等图像更适合采用JPG的图片格式保存。

2、PNG(Portable Network Graphics 便携式网络图片) 无损压缩，体积大，图片质量高，支持透明
png8 2^8色，文件较小，支持透明
png24 2^24色，不支持透明
png32 2^24色，支持透明
主要用于小图标或颜色简单对比强烈的小的背景图。页面结构的基本视觉元素，如容器的背景、按钮、导航的背景等应该尽量用PNG格式进行存储，这样才能更好的保证设计品质。

3、GIF(Graphics Interchange Format 图像互换格式) 无损耗性。最多支持256种色彩的图像.布尔透明，可以使全透明，也可是全不透明。支持动画。适合对颜色要求不高的图形（比如说图标、图表等）

4、WEBP 体积小，同时支持有损和无损压缩的、使用直接色的、点阵图，压缩程度更好，在 ios webview 兼容性问题

5、SVG(Scalable Vector Graphics) 矢量图，代码内嵌，相对较小，图片样式相对简单的场景

6、base64 将图片转换为base64编码字符串inline到CSS或页面中，适用于图片小于2KB、页面引用图片不多的情况，减少了http请求，数据就是图片。

7、canvas
需要高性能的图片或动画，使用HTML5的canvas元素绘制图片，页面渲染性能较高。

##### css 和 js 的装载与执行
HTML 渲染过程顺序执行、并发加载，每个域名并发度有限2-6，因此设置几个 CDN，或 gzip 压缩 如果使用 HTTP2，可以避开限制
CSS head 引入，防止闪动
CSS 和 js 都会阻塞 js 的执行，但不会阻塞外部资源的加载，因此可以预加载

##### 懒加载与预加载
懒加载: 监听scroll事件，图片进入可视区域，再请求资源，为图片赋上src。
预加载: 静态资源在使用之前提前请求，使用时可从缓存中加载
```html
1、 <img src="" style="display:none">
2、使用 Image 对象
var image = new Image();
image.src = "http://test.png";
3、XMLHttpRequest 能监听数据传输情况，但有跨域问题
4、PreloadJS
```

## 进阶

### 浏览器渲染
Html、JavaScript 和 CSS 在浏览器端的加载机制、重绘与回流渲染树的生成

#### 浏览器如何渲染网页
1、使用 HTML 创建文档对象模型（DOM）
浏览器从上到下读取标签，把他们分解成节点，从而创建 DOM 。
策略：样式在顶部，脚本在底部；最小化和压缩

2、使用 CSS 创建 CSS 对象模型（CSSOM）
CSSOM 的构建会阻塞页面的渲染
策略：延迟加载 CSS； 只加载需要的样式

3、基于 DOM 和 CSSOM 执行脚本（Scripts）
脚本只能等到先前的 CSS 节点构建完成。
策略：
异步加载脚本，添加 async 属性，可以通知浏览器不要阻塞其余页面的加载，下载脚本处于较低的优先级。一旦下载完成，就可以执行。使用于不影响 DOM 或 CSSOM 的脚本。
延迟加载脚本，defer 跟 async 非常相似，不会阻塞页面加载，但会等到 HTML 完成解析后再执行。

4、合并 DOM 和 CSSOM 形成渲染树（Render Tree）
一旦所有节点已被解析，DOM 和 CSSOM 准备合并，浏览器便会构建渲染树。如果我们把节点想象成单词，那么对象模型就是句子，渲染树便是整个页面。

5、使用渲染树布局（Layout）所有元素
布局阶段需要确定页面上所有元素的大小和位置。

6、渲染（Paint）所有元素
最终的渲染阶段，会真正地光栅化屏幕上的像素，把页面呈现给用户。

#### 回流与重绘
频繁触发重绘与回流，会导致UI频繁渲染，最终导致js变慢
回流必将引起重绘，而重绘不一定会引起回流

##### 回流
当render tree中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建。这就称为回流(reflow)。当页面布局和几何属性改变时就需要回流

盒子模型相关：width、height、padding、border、margin、display、border-width、min-height
定位属性及浮动：top、right、bottom、left、position、float、clear
节点内部文字结构，行内属性：text-align、overflow、line-height、vertical-align、font-weight、white-space、font-size

##### 重绘
当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。

外观属性: color、border-style、border-radius、visibility、background、box-shadow、outline

##### 避免重绘回流的两种方法
1、属性替代：避免使用触发重绘、回流的css属性
2、新建图层：将频繁重绘回流的dom元素单独作为一个独立图层，那么这个dom元素的重绘和回流的影响只会在这个图层中

##### 新建dom的过程
1、获取dom后分割为多个图层
2、对每个图层的节点计算样式结果（recalculate style-样式重计算）
3、为每个节点生成图形和位置（layout-回流和重布局）
4、将每个节点绘制填充到图层位图中（paint setup和paint-重绘）
5、图层作为纹理上传至gpu
6、符合多个图层到页面上生成最终屏幕图像（coposite layers-图层重组）

##### chrome创建图层的条件
3D或透视变换（perspective transform）CSS属性，比如transform:translateZ(0);
使用加速视频解码的 video节点
拥有3D（WebGL）上下文或加速的2D上下文的 canvas 节点
混合插件（如Flash）
对自己的opacity做CSS动画或使用一个动画webkit变换的元素
拥有加速CSS过滤器的元素
元素有一个包含复合层的后代节点（一个元素拥有一个子元素，该子元素在自己的层里）
元素有一个z-index较低且包含一个复合层的兄弟元素（换句话说就是该元素在复合层上面渲染）
video, canvas, will change:transform, overflow-scrolling:touch

##### 优化点
总体思路: 避免触发回流、重绘的 CSS 属性，或者 用 只触发重绘不回流；将回流。重绘的影响范围控制在单独的图层之内
1、translate 替换 top/left， 后者会触发回流但是translate不会
2、opacity 替换 visibility，visibility 会触发重绘但不会触发回流
opacity=0，该元素隐藏起来了，但不会改变页面布局，并且，如果该元素已经绑定一些事件，如click事件，那么点击该区域，也能触发点击事件的
visibility=hidden，该元素隐藏起来了，不会改变页面布局，但是不会触发该元素已经绑定的事件
display=none，把元素隐藏起来，并且会改变页面布局，可以理解成在页面中把该元素删除掉一样

3、不要一条一条修改dom样式，预先定义好class，然后修改dom的className
4、可以将dom离线操作，如先display:none(会有一次reflow)，但是接下来的操作都不会重绘，等离线操作结束后，再显示。
5、不要把dom节点的属性值放在循环里，当成循环的变量,在循环外面声明一个变量来中转。如offsetWidth/ offsetHeight，有强制刷新回流队列缓存，一定会触发回流
6、不要使用table，即使小改动，整个table也会回流
7、动画实现速度的选择，选择合适的动画的间隔时间
8、对于动画，可新建图层
9、使用gpu加速，比如加上transform：translateZ(0); transform：translate3d(0,0,0)

### 浏览器存储
LocalStorage、Cookie、IndexedDB、sessionStorage PWA、 Service Worker

#### cookie
1、cookie的初衷：因为http请求无状态，所以需要cookie去维持客户端状态。
2、cookie的生成方式：http response header中的set-cookie; js中可以通过document.cookie可以读写cookie
3、使用: 用于浏览器端和服务器端的交互; 客户端自身数据的存储
4、cookie存储数据能力被localStorage替代
5、httponly，当前cookie不支持js读写
6、cookie中在相关域名下面 cdn的流量损耗。cdn的域名和主站的域名要分开

#### indexDB
indexDB是一种低级API，用于客户端存储大量结构化数据。该api使用索引来实现对该数据的高性能搜索。虽然web storage对于存储少量的数据很有用，但对于存储更大量的结构化数据来说，这种方法很有用。indexDB提供了一个解决方案。
为应用创建离线版本。

#### PWA
PWA(progressive web apps)是一种web app新模型，并不是具体指某一种前沿的技术或者某一个单一的知识点，是一个渐进式的web app，是通过一系列新的web特性，配合优秀的ui交互设计，逐步的增强web app的用户体验。
1、方向
可靠：在没有网络的环境中也能提供基本的页面访问，而不会出现“未连接到互联网”的页面。
快速：针对网页渲染及网络数据访问有较好优化。
融入（engaging）：应用可以被增加到手机桌面，并且和普通应用一样有全屏、推送等特性。
2、检测pwa、检测性能：lighthouse

#### Service Worker
service worker是一个脚本，浏览器独立于当前网页，将其在后台运行，为实现一些不依赖页面或者用户交互的特性打开了一扇大门。在未来这些特性包括推送消息，背景后台同步，geofencing（地理围栏定位），但它将推出的第一个首要特性，就是拦截和处理网络请求的能力，包括以编程方式来管理被缓存的响应。

1、应用
使用拦截和处理网络请求的能力，去实现一个离线应用
使用service worker在后台运行同时能和页面通信的能力，去实现大规模后台数据的处理

2、查看
chrome://serviceworker-internals/、 chrome://inspect/#service-workers

3、注意事项
service workers只能在https下才能生成，本地调试能用localhost:80，不能用ip:80


### 缓存
缓存相关的浏览器和服务端能力

#### expires
缓存过期时间，用来指定资源到期的时间，是服务器端的具体的时间点。告诉浏览器在过期时间前浏览器可以直接从浏览器缓存取数据，而无需再次请求。HTTP 1.0协议中的。

如果在Cache-Control响应头设置了 "max-age" 或者 "s-max-age" 指令，那么 Expires 头会被忽略。
Expires: Thu, 11 Jul 2019 18:30:00 GMT
Expires: 0 , 无效的日期，代表着过去的日期，即该资源已经过期

#### Cache-Control 
通用消息头字段，被用于在http请求和响应中，通过指定指令来实现缓存机制。缓存指令是单向的，这意味着在请求中设置的指令，不一定被包含在响应中。HTTP1.1协议中的。

语法：指令不区分大小写，并且具有可选参数，可以用令牌或者带引号的字符串语法。多个指令以逗号分隔。

指令：
1、可缓存性
public，表明响应可以被任何对象（包括：发送请求的客户端，代理服务器，等等）缓存，即使是通常不可缓存的内容（例如，该响应没有max-age指令或Expires消息头）。
private，表明响应只能被单个用户缓存，不能作为共享缓存（即代理服务器不能缓存它）。私有缓存可以缓存响应内容。
no-cache，在发布缓存副本之前，强制要求缓存把请求提交给原始服务器进行验证，评估缓存响应的有效性。
no-store，禁止一切缓存

2、到期
max-age= seconds，设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒)。 200 (from memory cache)
s-maxage= seconds，覆盖 max-age 或者 Expires 头，但是仅适用于共享缓存(比如各个代理)，私有缓存会忽略它。 304

3、重新验证和重新加载
must-revalidate，一旦资源过期（比如已经超过max-age），在成功向原始服务器验证之前，缓存不能用该资源响应后续请求。
proxy-revalidate，与must-revalidate作用相同，但它仅适用于共享缓存（例如代理），并被私有缓存忽略。
immutable，表示响应正文不会随时间而改变。资源（如果未过期）在服务器上不发生改变，因此客户端不应发送重新验证请求头来检查更新，即使用户显式地刷新页面。

4、其他
no-transform，不得对资源进行转换或转变。Content-Encoding、Content-Range、Content-Type等HTTP头不能由代理修改。
only-if-cached，表明客户端只接受已缓存的响应，并且不要向原始服务器检查是否有更新的拷贝。


#### last-modified / if-modified-since
基于客户端和服务端协商的备用缓存机制，Last-Modified与Etag类似。不过Last-Modified表示响应资源在服务器最后修改时间而已。需要与cache-control共同使用。

不足: 
Last-Modified标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的修改时间；
如果某些文件会被定期生成，当有时内容并没有任何变化，但Last-Modified却改变了，导致文件没法使用缓存；
有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形。

Last-Modified: Thu, 11 Jul 2019 18:30:00 GMT , response header
if-modified-since: Thu, 11 Jul 2019 18:30:00 GMT , request header

####  etag / if-none-match
etag 实体标签，文件内容的hash值，服务器生成的一个标记，用来标识返回值是否有变化。
需要与cache-control共同使用，优先级比last-modified/ if-modified-since更高
etag是服务器自动生成或者由开发者生成的对应资源在服务器端的唯一标识符，能够更加准确的控制缓存。

当第一次发起请求时，服务器生成并会返回一个Etag给前端，并在你第二次发起同一个请求时，客户端会同时发送一个If-None-Match，而它的值就是Etag的值（此处由发起请求的客户端来设置）。然后，服务器会比对这个客服端发送过来的Etag是否与服务器的相同，如果相同，就将If-None-Match的值设为false，返回状态为304，客户端继续使用本地缓存，不解析服务器返回的数据（这种场景服务器也不返回数据），如果不相同，就将If-None-Match的值设为true，返回状态为200，客户端重新解析服务器返回的数据。

etag: "33a64df551425fcc55e4d42a148795d9f25f89d4" , response header
if-none-match: "33a64df551425fcc55e4d42a148795d9f25f89d4" , request header

#### 缓存机制
强制缓存优先于协商缓存进行，若强制缓存(Expires和Cache-Control)生效则直接使用缓存 200(from cache)，若不生效则进行协商缓存(Last-Modified / If-Modified-Since和Etag / If-None-Match)，协商缓存由服务器决定是否使用缓存，若协商缓存失效，那么代表该请求的缓存失效，返回200，重新返回资源和缓存标识，再存入浏览器缓存中；生效则返回304，继续使用缓存

![缓存机制](/images/web-cachemechanism.jpg)

## 服务器
### SSR
Server Side Rendering 解决首屏渲染问题  服务端是 nodejs，客户端复杂运算可以放到服务端，减少前端运算成本。


