---
title: 面试题 HTML和CSS
comments: true
date: 2019-03-22 10:07:06
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
（1）XSS(Cross-Site Scripting 跨站脚本攻击)。
含义：攻击者在网页中嵌入恶意脚本程序，运行非法的HTML标签或者JavaScript。
解决方案：将输入的数据进行转义处理；设置 HTTP Header： "X-XSS-Protection: 1"

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
