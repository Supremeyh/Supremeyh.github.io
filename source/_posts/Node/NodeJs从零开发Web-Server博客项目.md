---
title: Node.js从零开发Web Server博客项目
comments: true
date: 2019-05-24 17:00:00
categories: Node
tags: ['Node', 'FullStack']
---

> create by sea, 2019.5.7

## Node.js从零开发Web Server博客项目

### nodejs 介绍
#### 下载和安装
* 普通方式
访问官网 https://nodejs.org/en/ ，下载并安装, 命令行运行 
node -v  查看当前node版本  显示结果则安装成功
* nvm node版本管理工具 (macOS)
安装HomeBrew 
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
安装nvm
brew install nvm
查看当前所有node版本
nvm list 
nvm install node 安装最新版本
nvm install v10.15.3 安装指定版本
nvm use v10.13.0 切换到指定版本
nvm use --delete-prefix v10.15.3  切换到指定版本

#### nodejs和js的区别
ECMAScipt， 语法规范，定义了语法和词法
javascript， 使用了ECMAScipt语法规范，以及Web API(w3c标准)
nodejs， 使用了ECMAScipt语法规范，以及nodejs API

#### CommonJs 模块化
```JavaScript
// a.js
function add(a, b) {
  return a + b
}

function mul(a, b) {
  return a * b
}

module.exports = {
  add,
  mul
}


// b.js
const {add, mul} = require('./a')

const sum = add(2, 3)
const res = mul(2, 3)

console.log(sum, res)
```

#### debugger
npm init -y  初始化package.json
需指定package.json 对应 "main": "index.js"

#### server端和前端的区别, 切换思路
服务稳定性，使用PM2做进程守候
考虑内存和CPU (优化、扩展)， server端要承载很多请求，内存和CPU 都是稀缺资源。stream 写日志，使用redis 存session
日志记录, 记录、存储、分析日志，否则是盲人摸象
安全，如越权操作、数据库攻击、xss攻击
集群和服务拆分  


### 项目需求分析 和 技术方案
目标: 开发一个博客系统，具有博客的基本功能
需求：首页、作者主页、博客详情页；登录页；管理中心、新建页、编辑页

技术方案: 
数据如何存储(博客、用户)，数据库表设计
接口设计，如何与前端对接
登录，业界有统一的解决方案，一般不用再重新设计


### 开发博客项目之接口
####  http 概述
浏览器发送http请求过程: DNS解析(浏览器自身的DNS缓存且未过期、操作系统自身的DNS缓存且未过期、hosts文件、首选DNS服务器 运营商DNS、根域名服务器、顶级域名服务器 、域名注册商服务器) --> 发起TCP的3次握手 --> 建立TCP连接后发起http请求(报文格式为请求行、请求头部、请求包体) --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，并请求html代码中的资源 --> 浏览器对页面进行渲染呈现给用户

#### 处理http请求
```JavaScript
const http = require('http')
const querystring = require('querystring')

const server = http.createServer((req, res) => {
  const url = req.url

  // get请求与querystring
  if(req.method==='GET') {
    const path = url.split('?')[0]
    console.log(path) // 返回路由

    req.query = querystring.parse(url.split('?')[1])

    // 设置返回格式为JSON
    res.writeHead(200, {'Content-Type': 'application/json'})
    res.end(JSON.stringify(req.query))
  }

  // post请求与post data 
  if(req.method==='POST') {
    let postData = ''
    req.on('data', chunk => {
      postData += chunk.toString()
    })

    req.on('end', () => {
      console.log(postData)
      res.end('hello node')
    })
  }

})

server.listen(3000, () => {
  console.log('listening at http://localhost:3000')
})
```

#### 搭建开发环境
使用nodemon检测文件变化，自动重启node
使用cross-env 设置环境变量，兼容mac linux 和windows, 需先使用 npm i --save-dev cross-env 命令安装cross-env 


#### 开发接口，处理路由
初始化路由: 根据技术方案的设计，做出路由
返回假数据: 将路由和数据分离，以符合设计原则，使用 postman 处理http请求
结构层次: 划分为三层: www启动服务及基本配置、index分配路由、router处理路由、 controller接口调用获取结果、model格式化返回结果

启动服务，基本配置
```JavaScript
// /.bin/www.js
const http = require('http')

const PORT = 3000
const serverHandle = require('../index')

const server = http.createServer(serverHandle)
server.listen(PORT, () => {
  console.log('listening at http://localhost:3000')
})
```

分配路由
```JavaScript
// /index.js  
const quertstring = require('querystring')

const handleUserRouter = require('./src/router/user')
const handleBlogRouter = require('./src/router/blog')

// 处理 post body data
const getPostBodyData = (req) => {  
  return new Promise((resolve, reject) => {
    if(req.method !== 'POST') {
      resolve({})
    }
    if(!req.headers['content-type'].includes('application/json')) { 
    // 实际上req.headers['content-type'] 为 'application/json;charset=UTF-8'
      resolve({})
    }

    let postData = ''
    req.on('data', chunk => {
      postData += chunk.toString()
    })
    
    req.on('end', () => {
      if(!postData) {
        resolve({})
      }
      resolve(JSON.parse(postData))
    })
  })
}

const serverHandler = (req, res) => {
  // 设置返回数据格式 JSON
  res.setHeader('Content-Type', 'application/json')

  // 获取path 路由
  const url = req.url
  req.path = url.split('?')[0]

  // 解析query
  req.query = quertstring.parse(url.split('?')[1])

  // 处理 post body data
  getPostBodyData(req).then(postData => {
    req.body = postData    
    // 处理blog路由
    const blogData = handleBlogRouter(req, res)

    if(blogData) {
      res.end(
        JSON.stringify(blogData)
      )
      return
    }

    // 处理user路由
    const userData = handleUserRouter(req, res)
    if(userData) {
      res.end(
        JSON.stringify(userData)
      )
      return
    }

    // 未命中 404
    res.writeHead(404, {'Content-Type': 'text/plain'})
    res.write('404 Not Found')
    res.end()
  })

}

module.exports = serverHandler
```

处理路由，router/blog.js 和 router/user.js 分别处理blog和user相关路由
```JavaScript
// 如 router/user.js 
const handleUserRouter = (req, res) => {
  const { method, url, path } = req

  // 登录
  if(method==='POST' && path==='/api/user/login') {
    return {
      msg: '登录'
    }
  }
}

module.exports = handleUserRouter
```

格式化返回结果
```JavaScript
// model/resModel
class BaseModel {
  constructor(data, message) {
    // data是对象，message是字符串，并兼容不传data的情况
    if(typeof data === 'string') {
      this.message = data
      data = null
      message = null
    }

    if(data) {
      this.data = data
    }
    if(message) {
      this.message = message
    }
  }
}


class SuccessModel extends BaseModel {
  constructor(data, message) {
    super(data, message)
    this.errno = 0 
  }
}

class ErrorModel extends BaseModel {
  constructor(data, message) {
    super(data, message)
    this.errno = -1
  }
}


module.exports = {
  SuccessModel,
  ErrorModel
}
```

### MySql 数据存储
#### 安装
MySql 是web server中最流行的关系型数据库
官网下载安装MySql、mysql workbench 操作sql 的可视化客户端，或者使用Navicat

#### 操作数据库
* 建库
create schema 'myblog';  创建myblog 的数据库
show databases; 查询是否成功
* 建表
新建 users 和 blogs两个表
```JavaScript
// column datatype pk主键 nn不为空 AI自动增加 Default 
users: id username password realname
CREATE TABLE  `myblg`, `users` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `username` VARCHAR(20) NOT NULL,
  `password` VARCHAR(20) NOT NULL,
  `realname` VARCHAR(10) NOT NULL,
 PRIMARY KEY(`id`)
);

blogs: id title content createtime author
```
* 表操作
删改查
```JavaScript
use webserver;
-- show databases;

// 修改列属性 让id 自增, 或者选中fileds点击某列修改属性
alter table users change `id` `id` INT(10) NOT NULL AUTO_INCREMENT

// 增
insert into users (username, `password`, realname) values('zhangsan', '123', '张三');

// 查
select * from users;  // 所有列
select id, username from users  // 指定列名
select id, username from users where id='123' and username='zhangsan'; // 指定列名和条件
select id, username from users where username like '%zhang%'; // 模糊查询
select id, username from users where username like '%zhang%' order by id desc; // 排序 倒叙

// 改
update users set realname='张三', state=0 where username='lisi' limit 5

// 删
delete from users where username='lisi';
update users set state=0 where username='lisi'  // 实际项目中 更多修改表，新增state字段，通过修改状态标记是否可用，来软删除
```

#### nodejs操作数据库
npm i mysql --save
npm run dev 启动项目,这步需要安装cross-env, 使用命令npm i --save-dev cross-env

##### demo
```JavaScript
// myql-test/index.js  demo
const mysql = require('mysql')

// 创建连接对象
const con = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'sea123456',
  port: 3306,
  database: 'webserver'
})

// 开始连接
con.connect()

// 执行 sql 语句
const sql = `insert into users (username, realname, password, state) values('zl','赵六','123','1')`
con.query(sql, (err, result) => {
  if(err) {
    console.log(err)
    return
  }
  console.log(result)
  
})

// 关闭连接
con.end()
```

##### nodejs 连接 mysql 做成工具
在config/db.js 配置mysql, db/mysql.js 导出统一执行sql语句的函数 execSql。 这样就将mysql集成进项目，不再是上面的demo
```JavaScript
// config/db.js
const env = process.env.NODE_ENV

// 配置
let MYSQL_CONF = {}

if(env==='dev') {
  MYSQL_CONF = {
    host: 'localhost',
    user: 'root',
    password: 'sea123456',
    port: 3306,
    database: 'webserver'
  }
}

if(env==='production') {
  MYSQL_CONF = {//..}
}


module.exports = {
  MYSQL_CONF
}



// db/mysql.js
const mysql = require('mysql')
const { MYSQL_CONF } = require('../config/db')

// 创建连接对象
const con = mysql.createConnection(MYSQL_CONF)

// 开始连接
con.connect()

// 统一执行 sql 语句的函数
function execSql(sql) {
  return new Promise((resolve, reject) => {
    con.query(sql, (err, result) => {
      if(err) {
        reject(err)
      }
      resolve(result)
    })
  })
}

// 关闭连接  不需要关闭，单例模式
// con.end()

module.exports = {
  execSql
}
```

##### API对接mysql（博客列表）
替换假数据，controller改为mysql连接的真实数据, 并修改路由router 和 index.js 为异步promise。  以blog 的 getList 获取博客列表 为例
```JavaScript
// controller/blog.js
const getList = (author, keyword) => {
  let sql = `select * from blogs where 1=1 `
  if(author) {
    sql += `and author='${author}' `
  }
  if(keyword) {
    sql += `and title like '%${keyword}%' `
  }
  sql += `order by createtime desc;`
  let result = execSql(sql)
  return result
}


// router/blog.js
// 获取博客列表
if(method==='GET' && path==='/api/blog/list') {
  const author = req.query.author || ''
  const keyword = req.query.keyword || ''
  // const listData = getList(author, keyword)
  // return new SuccessModel(listData)
  const result = getList(author, keyword)
  return result.then(listData => {
    return new SuccessModel(listData)
  })
}

// index.js
getPostBodyData(req).then(postData => {
    req.body = postData    
    // 处理blog路由
    // const blogData = handleBlogRouter(req, res)
    // if(blogData) {
    //   res.end(
    //     JSON.stringify(blogData)
    //   )
    //   return
    // }
    const blogResult = handleBlogRouter(req, res)    
    if(blogResult) {
      blogResult.then(blogData => {
        res.end(
          JSON.stringify(blogData)
        )
      })
      return  // return 要写在这
    }
    // ..
}
```

### 登录校验、登录信息存储
#### cookie
HTTP Cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能。

主要用于: 会话状态管理（如用户登录状态、购物车）、个性化设置（如用户自定义设置、主题等）、浏览器行为跟踪（如跟踪分析用户行为等）

特点: 最大5kb、跨域不共享、格式如 k1=v1;k2=v2; 因此可以存储结构化数据

客户端操作cookie: document.cooke='k3=v3' 会累加到cookie中
server端操作cookie:   
```JavaScript
// index.js  解析cookie, 处理成键值对 存入req.cookie中
req.cookie = {}
const cookieStr = req.headers.cookie || ''
cookieStr.split(';').forEach(item => {
  if(!item) return
  const arr = item.split('=')
  const key = arr[0].trim()  // trim() 保证去除收尾空格
  const val = arr[1].trim()
  req.cookie[key] = val
})


// router/user.js
const handleUserRouter = (req, res) => {
  const { method, url, path } = req
  // 登录
  if(method==='POST' && path==='/api/user/login') {
    const { username, password } = req.body
    const result = loginCheck(username, password)
    return result.then(userData => {
      if(userData.username) {
        // 操作cookie  使用 HttpOnly 和 expires 限制前端获取改写和设置cookie及过期时间
        res.setHeader('Set-Cookie', `username=${userData.username}; path=/; HttpOnly; expires=${setCookieExpire()}`)
        return new SuccessModel(userData)
      }
      return new ErrorModel('登录失败')
    })
  }
}
```
####  session
cookie存储userid, server端的session存储用户信息
```JavaScript
// index.js
// session 数据
const SESSION_DATA = {}

// 解析 session
let needSetCookie = false
let userId = req.cookie.userid
if(userId) {
  if(!SESSION_DATA[userId]) {
    SESSION_DATA[userId] = {}
  }
} else {
  needSetCookie = true
  userId = `${Date.now()}_${Math.random()}`
  SESSION_DATA[userId] = {}
}
req.session = SESSION_DATA[userId]

// 处理 post body data
getPostBodyData(req).then(postData => {
  req.body = postData    
  // 处理blog路由
  const blogResult = handleBlogRouter(req, res)    
  if(blogResult) {
    blogResult.then(blogData => {
      // needSetCookie
      if(needSetCookie) {
        res.setHeader('Set-Cookie', `userid=${userId}; path=/; HttpOnly; expires=${setCookieExpire()}`)
      }
      res.end(
        JSON.stringify(blogData)
      )
    })
    return
  }
  // ...
}


// router/user.js
const handleUserRouter = (req, res) => {
  const { method, url, path } = req
  // 登录
  if(method==='POST' && path==='/api/user/login') {
    const { username, password } = req.body
    const result = loginCheck(username, password)
    return result.then(userData => {
      if(userData.username) {
        // 设置session
        req.session.username = userData.username
        
        // 操作cookie
        // res.setHeader('Set-Cookie', `username=${userData.username}; path=/; HttpOnly; expires=${setCookieExpire()}`)
        return new SuccessModel(userData)
      }
      return new ErrorModel('登录失败')
    })
  }
  // ...
}
```

#### session 写入redis
##### redis
Remote Dictionary Server是一个由Salvatore Sanfilippo写的key-value存储系统。
Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。
它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。

##### 为何将session 写入redis
问题：目前session是js变量，放在nodejs进程内存中。进程内存有限，访问量过大，可能内存暴增；正式线上运行是多进程，进程间无法共享。
方案：将web server和redis 拆分为两个单独的服务，双方都是独立可扩展的。
解释：redis是web server最常用的缓存数据库，数据存放在内存中，相比硬盘中的mysql，访问速度快快几个量级。但成本更高，可存储数据量更小，断电即消失。
原因: session访问频繁，对性能要求极高。session可忽略断电丢失数据的问题，重新获取即可。session相比mysql数据量不会太大。

##### redis 安装使用
安装：brew install redis
启动：redis-server、redis-cli  (6379是redis 服务端口)
设置：set myname sea  (在redis-cli中，下同)
获取： get myname
获取所有：keys *
删除： del myname

##### nodejs连接redis
redis-server 启动redis
npm i redis  安装redis依赖

```JavaScript
// redis-test demo
const redis = require('redis')

// 创建客户端
const redisClient = redis.createClient(6379, '127.0.0.1')

redisClient.on('error', err => {
  console.error(err)
})


redisClient.set('myname', 'sea', redis.print)
redisClient.get('myname', (err, val) => {
  if(err) {
    console.error(err)
    return
  }
  console.log('val', val)
  // 退出
  redisClient.quit()
})
```

##### nodejs连接redis 封装工具函数
```JavaScript
// config/db.js  配置redis常量
let REDIS_CONF = {}

if(env==='dev') {
  // redis
  REDIS_CONF = {
    host: '127.0.0.1',
    port: '6379',
  }
  // ...
}


// db/redis.js  封装成工具函数
const redis = require('redis')
const { REDIS_CONF } = require('../config/db')

// 创建redis客户端
const redisClient = redis.createClient(REDIS_CONF.port, REDIS_CONF.host)

// 监听error
redisClient.on('error', err => {
  console.error(err)
})

// 设置
function setRedisVal(key, val) {
  if(typeof val === 'object') {
    val = JSON.stringify(val)
  }
  redisClient.set(key, val, redis.print)
}

// 获取
function getRedisVal(key) {
  return new Promise((resolve, reject) => {
    redisClient.get(key, (err, val) => {
      if(err) {
        reject(err)
        return
      }
      if(val===null) {
        resolve(null)
        return
      }
      try {
        // 优先返回JSON 格式
        resolve(JSON.parse(val))
      } catch(e) {
        resolve(val)
      }
      // redisClient.quit()
    })
  })
}

module.exports = {
  setRedisVal,
  getRedisVal,
}
```
##### session登录验证 redis
```JavaScript
// index.js
const { setCookieVal } = require('./src/utils')
const { setRedisVal, getRedisVal } = require('./src/db/redis')

const serverHandler = (req, res) => {
  // 设置返回数据格式 JSON
  res.setHeader('Content-Type', 'application/json')

  // 获取path 路由
  const url = req.url
  req.path = url.split('?')[0]

  // 解析query
  req.query = quertstring.parse(url.split('?')[1])
  
  // 解析cookie  处理成键值对 存入req.cookie中
  const cookie = parseCookie(req)
  req.cookie = cookie

  // 解析 session
  let needSetCookie = false
  let userId = req.cookie.userid
  if(!userId) {
    needSetCookie = true
    userId = `${Date.now()}_${Math.random()}`
  }

  // 将 sessionId 设置为 userId，后面的登录路由处理会用到
  req.sessionId = userId

  // 通过 userId 获取存储在 redis 中的数据
  getRedisVal(req.sessionId)
    .then(sessionData => {
      if(sessionData===null) {
        // 当对应的 sessionId 在 redis 中没有值的时，在 redis 中将其值设置为空对象
        setRedisVal(req.sessionId, {})
        req.session = {}
      } else {
        req.session = sessionData
      }
      return getPostBodyData(req)
    })
    .then(postData => {
      req.body = postData    
      // 处理blog路由
      const blogResult = handleBlogRouter(req, res)    
      if(blogResult) {
        blogResult.then(blogData => {
          if(needSetCookie) {
            setCookieVal(res, 'userid', userId)
          }
          res.end(
            JSON.stringify(blogData)
          )
        })
        return
      }
      // ...
  })
}


// utils/index.js 统一的登录验证函数
const loginCheckSession = (req) => {
  // session中没有username信息，则返回错误信息
  if(!req.session.username) {
    return  Promise.resolve(new ErrorModel('未登录'))
  }
  // return undefined  // 这行可注释掉，即没有返回值，或返回undefined
}


// router/blog.js
const { loginCheckSession } = require('../utils')

const handleBlogRouter = (req, res) => {
  const { method, url, path } = req

  // 登录验证  放在handleBlogRouter的顶部做成拦截器，保证所有后期请求都需先登录
  const loginCheckResult = loginCheckSession(req)
  if(loginCheckResult) {  // 有值，说明未登录
    return loginCheckResult
  }

  // 新建一篇博客  比如新建博客时，需要校验author，从session的username取出，保证登录的账号和author是同一个人，不会删除其他人的博客
  if(method==='POST' && path==='/api/blog/new') {
    const author = req.session.username
    req.body.author = author
    
    const blogData = req.body
    const dataResult = newBlog(blogData)
    return dataResult.then(data => {
      return new SuccessModel(data)
    })
  }
  // ...

}


// router/user.js
const { setRedisVal } = require('../db/redis')

const handleUserRouter = (req, res) => {
  const { method, url, path } = req
  // 登录
  if(method==='POST' && path==='/api/user/login') {
    const { username, password } = req.body
    const result = loginCheck(username, password)
    return result.then(userData => {
      if(userData.username) {
        // 设置session
        req.session.username = userData.username
        setRedisVal(req.sessionId, req.session)
        return new SuccessModel(userData)
      }
      return new ErrorModel('登录失败')
    })
  }
}
```
##### 前后端联调
登录功能依赖cookie，需要userid，必须要和浏览器联调
cookie跨域不共享，前后端必须同域，使用nginx 做代理，让前后端同域
开发前端网页

##### nginx 
高性能web服务器，开源免费。一般用于做静态服务、负载均衡、反向代理服务器。
下载:mac brew install nginx 
配置: /usr/local/etc/nginx/nginx.conf  (Mac) ，用vi 命令打开
测试配置文件是否正确: nginx -t
启动: nginx -s reload
停止: nginx -s stop
```JavaScript
// /usr/local/etc/nginx/nginx.conf
http {
  server {
    # 当访问 http://localhost:8080... 时生效
    listen        8080;
    server_name   localhost;
    
    # location / {
    #  root       html;
    #  index      index.html index.htm;
    # }

    # 静态文件 前端项目启动的端口
    location / {
      proxy_pass  http://localhost:8000;
      # 使用Nginx对WebSocket进行反向代理, 表示请求服务器升级协议为WebSocket
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "Upgrade";
    }

    # server 后端项目启动的端口
    location /api/ {
      proxy_pass  http://localhost:3000;  
      proxy_set_header Host $host;
    }
  }

}
```
配置完成后，前端启动服务(如端口8000)，后端启动服务(端口3000)，浏览器访问 http://localhost:8080... 即可。


### 日志 
系统没有日志，就等于人没有眼睛
分类: 访问日志 access log、自定义日志(自定义事件、错误记录等)
nodejs文件操作，nodejs stream
日志拆分、内容分析

#### nodejs文件操作
```JavaScript
// file-test demo
// file-test/test.js
const fs = require('fs')
const path = require('path')

const fileName = path.resolve(__dirname, 'data.txt')  // file-test/data.txt

// 读
fs.readFile(fileName, (err, data) => {
  if(err) {
    console.error(err)
    return
  }
  // data是二进制数据  <Buffer 68 65 6c 6c 6f 20 0a 7a 7a 6c 61 70 61 0a 70 61 70 61 61 2d 2d 0a 7a 7a 7a 61>
  console.log(data.toString())
})

// 写
let content = 'new ...'
const opt = {
  flag: 'a',  // 追加a append, 覆盖w write
}

fs.writeFile(fileName, content, opt, (err) => {
  if(err) {
    console.error(err)
  }

})

// 判断文件是否存在
fs.exists(fileName, (exist) => {
  console.log('exist', exist)  // true
})
```
#### stream
IO操作(网络、文件)的性能瓶颈,突出特点就是慢
标准输入输出  process.stdin.pipe(process.stdout)
req.pipe(res) 类比成两个水桶，连接管道
```JavaScript
// file-test demo
// file-test/test.js
const fs = require('fs')
const path = require('path')

const fileName1 = path.resolve(__dirname, 'data.txt')
const fileName2 = path.resolve(__dirname, 'data-bak.txt')

const readStream = fs.createReadStream(fileName1)
const writeStream = fs.createWriteStream(fileName2)

readStream.pipe(writeStream)

readStream.on('data', chunk => {
  console.log(chunk.toString())
})

readStream.on('end', () => {
  console.log('ok')
})
```
#### 写日志
目录logs下，并新建 access.log、event.log、error.log 三个文件
```JavaScript
// utils/log.js  
const fs = require('fs')
const path = require('path')


function createWriteStream (fileName) {
  const fullName =  getFullName(fileName)
  const writeStream = fs.createWriteStream(fullName, {
    flags: 'a'
  })
  return writeStream
}

function writeLog(writeStream, log) {
  writeStream.write(log + '\n')
}


// 获取完整文件路径名
function getFullName(fileName) {
  const fullName = path.join(__dirname, '../', 'logs', fileName)  
  return fullName
}

// 访问日志
const accessWriteStream = createWriteStream('access.log')

function writeAccessLog(log) {
  writeLog(accessWriteStream, log)
}

module.exports = {
  writeAccessLog
}


// index.js
const { writeAccessLog } = require('./src/utils/log')

const serverHandler = (req, res) => {
  // 记录 access log 
  writeAccessLog(`${req.method}--${req.url}--${req.headers['user-agent']}--${Date.now()}`)
  // ...
}
```
之后，刷新页面，即可把请求写到日志

#### 日志拆分 crontab
日志慢慢积累，放在一个文件不好处理，一般按时间拆分日志，如2019-05-13.access.log
实现方式: linux的crontab命令，即定时任务
crontab设置定时任务，格式: `*****command` 分-时-日-月-星期几
流程：将access.log拷贝并重命名为2019-05-13.access.log，清空access.log文件，继续积累日志
创建utils/copy.sh脚本
```sh
#!/bin/sh
cd /Users/sea/Desktop/OnGoing/HelloWorld/FullStack/Node/webserver/src/logs
cp access.log $(date +%Y-%m-%d).access.log
echo "" > access.log
```
cli 执行 sh src/utils/copy.sh 即可实现将access.log日志拆分到 2019-05-13.access.log

cli 执行 crontab -e ，会打开编译器，输入
```Javascript
* 0 * * * sh /Users/sea/Desktop/OnGoing/HelloWorld/FullStack/Node/webserver/src/utils/copy.sh
```
wq保存，显示crontab:installing new crontab， 则完成每天凌晨0点0分 执行日志拆分脚本
crontab -l 查看当前任务列表

#### 日志分析 readline
比如针对access.log 日志，分析使用chrome的占比
日志按行存储，一行就是一条日志
使用nodejs的readline(基于stream，效率高)
```Javascript
// utils/readline.js
const fs = require('fs')
const path = require('path')
const readline = require('readline')

// 获取access.log 文件路径名
const fileName = path.join(__dirname, '../', 'logs', 'access.log')
// 创建 read stream
const readStream = fs.createReadStream(fileName)

// 创建readline对象
const rl = readline.createInterface({
  input: readStream
})

let total = 0
let chromeNum = 0

// 逐行读取
rl.on('line', lineData => {
  if(!lineData) return
  total++

  let arr = lineData.split('--')
  if(arr[2] && arr[2].includes('Chrome')) {
    chromeNum++
  }
})

// 监听读取完成
rl.on('close', () => {
  console.log('使用chrome占比 ' + chromeNum/total)
})
```

### 安全
#### xss 攻击: 窃取前端的cookie
方式: 在页面展示内容中掺杂js代码，以获取网页信息
预防: 转换生成js的特殊字符
```Javascript
& ->  &amp;
< ->  &lt;
> ->  &gt;
' ->  &#x27;
" ->  &quot;
```
使用xss库
npm i xss --save
```Javascript
// controller/blog.js
const xss = require('xss')

const newBlog = (blogData={}) => {
  const { title, content, author, createtime } = blogData
  // 防止xss攻击
  title = xss(title)
  // ...
}
```

#### sql 注入: 窃取数据库内容
最原始、最简单的攻击，自从web2.0就有了sql注入攻击
方式:输入一个sql片段，最终拼接成一段攻击代码
预防: 使用mysql的escape函数处理输入内容即可
```JavaScript
// controller/user.js 
// 登录时账号密码输入如下（在mysql--表示注释）
select * from users where username='zhangsan';delete from users; -- 'and password='123';
```
上面代码发生sql注入。使用mysql的escape，并去掉变量的双引号
```JavaScript
// db/mysql.js
module.exports = {
  execSql,
  escape: mysql.escape
}

// controller/user.js
const { execSql, escape } = require('../db/mysql')
// 加上
username = escape(username)
password = escape(password)

// let sql = `select * from users where username='${username}' and password='${password}';`
let sql = `select * from users where username=${username} and password=${password};`  // 去掉变量的双引号
```
#### 密码加密: 保证用户信息安全
万一数据库被攻破，最不该泄露的就是用户账号密码信息
方式: 是用户账号密码，再去登录其他系统
预防: 密码加密，不使用明文
```JavaScript
// utils/crypto.js
const crypto = require('crypto')
// 密钥
const SECRET_KEY = 'HELLO_Node@2019'
// md5 加密
function md5(content) {
  let md5 = crypto.createHash('md5')
  return md5.update(content).digest('hex')
}
// 加密函数
function generatePassword(password) {
  const str = `password=${password}&key=${SECRET_KEY}`
  return md5(str)
}

module.exports = {
  generatePassword
}

// 更新数据库所有用户密码为密文（其实应该在注册时就对密码加密，让注册和登录对上，此处在数据库编译器里执行只是测试加密）
update users set `password`='a04db8f90de6663c06460b383ad78eae'

// controller/user.js  
const { generatePassword} = require('../utils/crypto')

const loginCheck = (username, password) => {
  // 密码加密  注意要先加密再escape的顺序，不然sql的password单引号丢失，导致sql语句执行错误
  password = generatePassword(password)

  // 使用mysql的escape函数 防止sql注入
  username = escape(username)
  password = escape(password)

  let sql = `select * from users where username=${username} and password=${password};`
  // ...
}
```

#### 原生开发总结
流程图：客户端 -> nginx反向代理 -> / server静态文件、/api/ ... -> 日志记录stream、crontab -> 路由处理 -> 登录校验redis -> 用户信息redis -> 数据处理controller -> 数据存储mysql

开启服务: 启动后端服务3000端口、启动redis-server、启动nginx、启动数据库mysql、启动前端页面8000端口，打开前端页面访问http 8080端口


### 使用 express 重构博客项目
express 是nodejs最常用的web server框架， Fast, unopinionated, minimalist web framework for node.


#### 下载、安装
express-generator  使用脚手架
```JavaScript
npm i express-generator -g  // 全局安装脚手架
express blog-express  // 生成项目

npm i cross-env --save-dev // 安装cross-env

// 配置package.json (部分)
"scripts": {
  "start": "node ./bin/www",
  "dev": "cross-env NODE_ENV=development nodemon ./bin/www",
  "prod": "cross-env NODE_ENV=production nodemon ./bin/www"
},
"devDependencies": {
  "cross-env": "^5.2.0"
}

npm i  // 安装依赖
npm run dev // 启动项目
```

#### 开发接口
##### 初始化环境、路由
安装插件 mysql、xss
config、mysql、controller、model/resModel相关代码可以复用blog-origin原生开发代码，要保证相关资源的引入和路径修改

记得启动nginx、mysql 和 redis-server

##### 处理session
使用express-session和connect-redis，简单方便
req.session保存登录信息，登录校验做成express中间件

npm i express-session --save
```JavaScript
// app.js
var session = require('express-session')
var { SECRET_KEY } = require('./utils/crypto')

// session  在路由前面配置
app.use(session({
  secret: SECRET_KEY,
  cookie: {
    path: '/',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000
  }
}))
```

##### 使用session登录
```JavaScript
// routes/login.js
const express = require('express')
const router = express.Router()
const { loginCheck } = require('../controller/login')
const { SuccessModel, ErrorModel } = require('../model/resModel')

router.post('/login', (req, res, next) => {
  // 登录
  const { username, password } = req.body
  const loginResult = loginCheck(username, password)
  return loginResult.then(userData => {      
    if(userData.username) {
      // 设置 session  会自动同步到redis
      req.session.username = userData.username
              
      // 同步到 redis  这步不需要了
      // setRedisVal(req.sessionId, req.session)

      // return new SuccessModel(userData, '登录成功')
      // 改成, 失败下同
      res.json(new SuccessModel(userData, '登录成功'))
      return
    }
    res.json(new ErrorModel('登录失败'))
  })

})

module.exports = router
```
其中，setRedisVal 不再需要,  设置 session时 会自动同步到redis。返回结果改成res.json() 在return的方式。
之后，使用postman，参数输入账号密码可测试登录接口， 如zhangsan, 123 返回登录成功。 或者，启动nginx 通过前端服务访问。

##### 连接redis, session存入redis
npm i redis connect-redis --save
```JavaScript
// db/redis.js  创建redisClient
const redis = require('redis')
const { REDIS_CONF } = require('../config/db')

// 创建redis客户端
const redisClient = redis.createClient(REDIS_CONF.port, REDIS_CONF.host)

// 监听error
redisClient.on('error', err => {
  console.error(err)
})

module.exports = redisClient


// app.js
var redisClient = require('./db/redis')
var redisStore = require('connect-redis')(session)

// session
const sessionStore = new redisStore({
  client: redisClient
})
app.use(session({
  secret: SECRET_KEY,
  cookie: {
    path: '/',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000
  },
  store: sessionStore  // 存入store
}))
```
这步要启动redis-server

查看redis: 启动redis-cli -> 先清空flushdb -> keys * 返回为空 -> "sess:W..y " ->  get sess:W..y 没有username->启动前端8081 -> nginx代理 8080登录 -> get sess:W..y 有username

##### 登录中间件
新建middleware/loginCheckSession.js 文件, loginCheckResult中间件，判定session中是否有username。 之后，在需要登录校验的接口调用中间件。
```JavaScript
// middleware/loginCheckSession   loginCheckResult中间件
const { ErrorModel } = require('../model/resModel')

const loginCheckSession = (req, res, next) => {
  if(req.session.username) {
    next()
    return
  }
  res.json(new ErrorModel('未登录'))
}

module.exports = loginCheckSession


// routes/blog.js  调用
const loginCheckSession = require('../middleware/loginCheckSession')

router.get('/list', loginCheckSession, (req, res, next) => {
  let author = req.query.author
  const keyword = req.query.keyword
  // 管理员界面
  if(req.query.isadmin) {
    if(req.session.username==null) {
      res.json(new ErrorModel('未登录'))
      return
    }
    // 强制只查询登陆用户自己的博客
    author = req.session.username
  }
  // ...

})
```

##### 开发路由，获取博客详情等
与以上登录类似，以获取博客详情为例, 更新、删除等类似
```JavaScript
// routes/blog.js
// 获取博客详情
router.get('/detail', (req, res, next) => {
  const id = req.query.id
  let detailResult = getDetail(id)
  return detailResult.then(detailData => {
    res.json(new SuccessModel(detailData))
  })
})
```

##### 记录日志
access log记录日志，直接使用脚手架推荐的morgan
```JavaScript
// app.js
var path = require('path')
var fs = require('fs')
var logger = require('morgan')


var ENV = process.env.NODE_ENV
if(ENV !=='production') {
  // 开发、测试环境
  app.use(logger('dev', {
    stream: process.stdout  // 打印到控制台  可省略
  }))
} else {
  // 线上环境
  const fileName = path.join(__dirname, 'logs', 'access.log')
  const writeStream = fs.createWriteStream(fileName, {
    flags: 'a'
  })
  app.use(logger('combined', {
    stream: writeStream
  }))
}
```

##### express 中间件原理
app.use() 注册中间件，先收集起来
遇到http请求，根据method和path判断触发哪些
实现next机制，即上一个通过next触发下一个

express实现原理
```JavaScript
// lib/like-express.js 
const http = require('http')
const slice = Array.prototype.slice

class LikeExpress {
  constructor() {
    // 存放中间件列表
    this.routes = {
      all: [],
      get: [],
      post: []
    }
  }

  register(path) {
    const info = {}
    if(typeof path === 'string') {
      info.path = path
      // 从第二个参数开始，转换为数组，存入stack
      info.stack = slice.call(arguments, 1)
    } else {
      info.path = '/'
      // 从第一个参数开始，转换为数组，存入stack
      info.stack = slice.call(arguments, 0)
    }
    return info
  }

  use() {
    const info = this.register.apply(this, arguments)
    this.routes.all.push(info)
  }

  get() {
    const info = this.register.apply(this, arguments)
    this.routes.get.push(info)
  }

  post() {
    const info = this.register.apply(this, arguments)
    this.routes.post.push(info)
  }

  match(method, url) {
    let stack = []
    if(url==='/favicon.ico') {
      return stack
    }
    // 获取routes
    let curRoutes = []
    curRoutes = curRoutes.concat(this.routes.all, this.routes[method])
    
    curRoutes.forEach(routeInfo => {
      if(url.indexOf(routeInfo.path) === 0) {
        // url==='/api/test' 且 routeInfo.path为 '/'、'/api' 或 '/api/test'
        stack = stack.concat(routeInfo.stack)
      }
    })
    return stack
  }

  // next 机制
  handle(req, res, stack) {
    // 定义next，并立即执行
    const next = () => {
      // 拿到第一个匹配的中间件
      const middleware = stack.shift()
      if(middleware) {
        // 执行中间件函数
        middleware(req, res, next)
      }
    }
    next()
  }

  callback() {
    return (req, res) => {
      res.json = (data) => {
        res.setHeader('Content-Type', 'application/json')
        res.end(JSON.stringify(data))
      }
      const url = req.url
      const method = req.method.toLowerCase() // 这里必须 toLowerCase() 统一成小写
      const resultList = this.match(method, url)
      this.handle(req, res, resultList)
    }
  }

  listen(...args) {
    const server = http.createServer(this.callback())
    server.listen(...args)
  }
}


// 工场函数
module.exports = () => {
  return new LikeExpress()
}
```

测试代码, 测试自己实现的express 是否生效
```JavaScript
// lib/express/test-express
const express = require('./like-express')
const app = express()

app.use((req, res, next) => {
  console.log('开始')
  next()
})

app.use((req, res, next) => {
  console.log('cookie')
  req.cookie = {
    userId: 'abc'
  }
  next()
})

app.use('/api', (req, res, next) => {
  console.log('use api')
  next()
})

app.get('/api', (req, res, next) => {
  console.log('get api')
  next()
})

loginCheck = (req, res, next) => {
  setTimeout(() => {
    console.log('loginCheck')
    next()
  }, 1000);
}

app.get('/api/test', loginCheck, (req, res, next) => {
  console.log('get api test')
  res.json({
    errorno: 0,
    data: req.cookie
  })
})


app.listen(3001, () => {
  console.log('listening at http://localhost:3001')
})  
```
运行 node lib/express/test-express 访问页面，查看控制台输出


##### 小结
使用express框架与原生开发区别:
写法上发生改变，如req.query、req.json，可直接获取写入，不需要自己写入； 
存储session到redis 使用 express-session、connect-redis，登录中间件;
记录日志，使用morgan
express原理


### 使用 Koa2 重构博客项目
express 中间件是异步回调，koa2原生支持async/await
新开发框架都开始基于koa，例如egg.js
express 虽然未过时，但koa2肯定是未来趋势
注: node版本大于等于8.0

#### async/await 异步函数介绍
##### 异步方式对比
在/test/files下有a.json、b.json、c.json三个文件，分别用callback、promise 和 async/await三种方式实现异步获取
* callback-hell
```JavaScript
// /test/index.js
const fs = require('fs')
const path = require('path')

// callback
function getFileContentByCb(fileName, cb) {
  const fullFileName = path.resolve(__dirname, 'files', fileName)
  fs.readFile(fullFileName, (err, data) => {
    if(err) {
      console.error(err)
      return 
    }
    cb(JSON.parse(data.toString()))
  })
}

// test callback-hell
getFileContentByCb('a.json', aData => {
  console.log('a data', aData)
  getFileContentByCb(aData.next, bData => {
    console.log('b data', bData)
    getFileContentByCb(bData.next, cData => {
      console.log('c data', cData)
    })
  }) 
})

// 打印结果
// a data { msg: 'this is a', next: 'b.json' }
// b data { msg: 'this is b', next: 'c.json' }
// c data { msg: 'this is c', next: null }
```
* promise
```JavaScript
// /test/index.js
function getFileContentByPromise(fileName) {
  const fullFileName = path.resolve(__dirname, 'files', fileName)
  return new Promise((resolve, reject) => {
    fs.readFile(fullFileName, (err, data) => {
      if(err) {
        reject(err)
        return
      }
      resolve(JSON.parse(data.toString()))
    })
  })
}

// promise().then()
getFileContentByPromise('a.json')
  .then(aData => {
    console.log('a data', aData)
    return getFileContentByPromise(aData.next)
  })
  .then(bData => {
    console.log('b data', bData)
    return getFileContentByPromise(bData.next)
  })
  .then(cData => {
    console.log('c data', cData)
  })
```
* async/await 变成同步的写法
要点:
await包裹在async函数里面； 
await后可追加promise对象； 
async函数执行返回的也是一个promise对象；
promise中resolve内容可以被await解析返回到前面的变量中； 
try...catch 截获promise中reject的值
```JavaScript
// /test/index.js   
async function readFileData() {
  try {
    const aData = await getFileContentByPromise('a.json')  // 其中，getFileContentByPromise和promise一样。
    console.log('a data', aData)
    const bData = await getFileContentByPromise(aData.next)
    console.log('b data', bData)
    const cData = await getFileContentByPromise(bData.next)
    console.log('c data', cData)
  } catch(e) {
    console.error(e)
  }
}

readFileData()
```

#### koa介绍、安装、使用
Expressive middleware for node.js using ES2017 async functions
```JavaScript
npm i koa-generator -g  // 安装脚手架
Koa2 blog-koa  // 生成blog-koa 的项目
npm i cross-env --save-dev  // 安装 cross-env

// package.json  修改配置
"scripts": {
  "start": "node bin/www",
  "dev": "cross-env NODE_ENV=dev ./node_modules/.bin/nodemon bin/www",
  "prd": "cross-env NODE_ENV=production pm2 start bin/www",
},

npm i && npm run dev  // 安装依赖、启动项目

// http://localhost:3000/   // 打开浏览器访问默认3000端口
```

#### koa 路由
需要特别注意的是，之前的req.body 都需要改为 ctx.request.body 请求体，以区分ctx.body 返回体
```JavaScript
// app.js
const login = require('./routes/login')
// routes
app.use(login.routes(), login.allowedMethods())


// routes/login.js
const router = require('koa-router')()

router.prefix('/login')

router.post('/login', async (ctx, next) => {
  const { username, password } = ctx.request.body
  ctx.body = {
    code: 2000,
    username,
    password,
  }
})

module.exports = router
```
#### koa 中间件
app.use()注册中间件，使用async/await; next() 也是一个promise
```JavaScript
// app.js  以 logger 中间件为例
app.use(async (ctx, next) => {
  const start = new Date()
  await next()
  const ms = new Date() - start
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`)
})
```
#### 实现登录 redis存储session
实现登录的session， 和express类似，基于koa-generic-session 和 koa-redis
npm i koa-generic-session redis koa-redis --save
```JavaScript
// app.js   配置session、redis
const session = require('koa-generic-session')
const redisStore = require('koa-redis')
const { REDIS_CONF } = require('./config/db')

// session  在logger和routes之间  
app.keys = ['HELLO_Node@2019']
app.use(session({
  // cookie
  cookie: {
    path: '/',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000
  },
  // redis
  store: redisStore({
    // all: '127.0.0.1:6379'  // 写死
    all: `${REDIS_CONF.host}:${REDIS_CONF.port}` // 配置store中all为 REDIS_CONF 配置的参数
  })
}))


// routes/login.js   测试
router.get('/test', async(ctx, next) => {
  if(ctx.session.viewNum==null) {
    ctx.session.viewNum = 0
  }
  ctx.session.viewNum++ 

  ctx.body = {
    code: 2000,
    viewNum: ctx.session.viewNum
  }
})
```
启动redis-server， 浏览器访问 http://localhost:3000/login/test  测试; 或者 redis-cli -> keys*  -> get ...  方式来测试

#### 开发路由
##### 准备工作
npm i mysql xss --save  安装 mysql、xss 依赖

复用blog-express之前代码，如登录中间件 config、db/mysql.js、controller、utils、middleware、model 等文件夹或文件 到blog-koa项目，对其中一些文件进行修改，中间件要符合koa中间件形式

修改controller 为 async/await 函数。 此处举getDetail、newBlog两个例子，可对比promise 和 async/await 的用法区别，写法清晰很多
```JavaScript
// controller/blog.js  以getDetail函数为例
const getDetail = (id) => {
  let sql = `select * from blogs where id='${id}';`
  return execSql(sql).then(arr => {
    return arr[0]
  })
}
// 修改为
const getDetail = async (id) => {
  let sql = `select * from blogs where id='${id}';`
  const rows = await execSql(sql)
  return rows[0]
}


// controller/blog.js  再以newBlog函数为例
const newBlog = async (blogData={}) => {
  // ...
  return execSql(sql).then(insertData => {
    return {
      id: insertData.insertId
    }
  })
}
// 修改为
const newBlog = async (blogData={}) => {
  // ...
  const insertData = await execSql(sql)
  return {
    id: insertData.insertId
  }
}
```
修改 middleware/loginCheckSession.js  改promise方式为koa方式。 这个是比较标准的koa中间件。
```JavaScript
// middleware/loginCheckSession.js 
const { ErrorModel } = require('../model/resModel')

const loginCheckSession = (req, res, next) => {
  if(req.session.username) {
    next()
    return
  }
  res.json(new ErrorModel('未登录'))
}

module.exports = loginCheckSession

// 修改为
const { ErrorModel } = require('../model/resModel')

const loginCheckSession = async (rctx, next) => {
  if(ctx.session.username) {
    await next()
    return
  }
  ctx.body = new ErrorModel('未登录')
}

module.exports = loginCheckSession
```
##### 初始化路由，开发接口
开发routes/login.js和routes/blog.js路由，这步也可以复用之前blog-express中routes下的代码，对其进行改造koa的async/await形式。以routes/blog.js 中获取博客详情为例，
```JavaScript
// routes/blog.js
// controller  这几行可直接复用
const { getList, getDetail, newBlog, updateBlog, delBlog } = require('../controller/blog')
// model
const { SuccessModel, ErrorModel } = require('../model/resModel')
const loginCheckSession = require('../middleware/loginCheckSession')

// 以下需改造
router.get('/detail', (req, res, next) => {
  const id = req.query.id
  let detailResult = getDetail(id)
  return detailResult.then(detailData => {
    res.json(new SuccessModel(detailData))
  })
})
// 替换为
router.get('/detail', async (ctx, next) => {
  const id = ctx.query.id
  const detailData = await getDetail(id)
  ctx.body = new SuccessModel(detailData)
})
```
##### 联调测试，连接数据库，启动nginx、前端项目
前端页面访问请求，或者使用postman模拟

#### 日志记录、拆分与分析
access log 记录，使用morgan, 新建logs/access.log文件
koa框架中koa-logger仅仅使得日志打印格式化，并未真正记录日志，需借助koa-morgan, 因morgan只适用于express
自定义日志暂时使用console.log/error

npm i koa-morgan --save  安装依赖 koa-morgan
```JavaScript
// app.js
const path = require('path')
const fs = require('fs')
const morgan = require('koa-morgan')

// logs  可写在logger之后。类似于express,但需app.use(logger('dev'))改为app.use(morgan('dev')),因logger已被占用
const ENV = process.env.NODE_ENV
if(ENV !=='production') {
  // 开发、测试环境
  app.use(morgan('dev'));
} else {  
  // 线上环境
  const fileName = path.join(__dirname, 'logs', 'access.log')
  const writeStream = fs.createWriteStream(fileName, {
    flags: 'a'
  })
  app.use(morgan('combined', {
    stream: writeStream
  }));
}
```

#### koa2中间件原理
##### koa2官方实例
洋葱圈模型: request -> onion-1 start -> onion-2 start -> onion-2 end -> onion-1 end -> response
```JavaScript
// blog-koa/lib/test-koa.js   nodemon test-koa.js 启动
const Koa = require('koa');
const app = new Koa();

// logger
app.use(async (ctx, next) => {
  console.log('onion-1 ', 'start')
  
  await next();
  const rt = ctx.response.get('X-Response-Time');
  console.log(`${ctx.method} ${ctx.url} - ${rt}`);

  console.log('onion-1 ', 'end')
});

// x-response-time
app.use(async (ctx, next) => {
  console.log('onion-2 ', 'start')

  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  ctx.set('X-Response-Time', `${ms}ms`);

  console.log('onion-2 ', 'end')
});

// response
app.use(async ctx => {
  console.log('onion-3 ', 'start')

  ctx.body = 'Hello World';

  console.log('onion-3 ', 'end')
});

app.listen(3000);
```
##### 原理分析
app.use 注册中间件，先收集起来
实现next机制，即上一个通过next触发下一个
不涉及method和path判断

##### 核心代码
```JavaScript
// lib/like-koa.js
const http = require('http')

// 组合中间件
function compose(middlewareList) {
  return function(ctx) {
    // 中间件调用
    function dispatch(i) {
      const fn = middlewareList[i]
      // 兼容未使用async，保证返回的中间件都是promise
      try {
        return Promise.resolve(
          fn(ctx, dispatch.bind(null, i+1))  // next机制
        )
      } catch (e) {
        Promise.reject(e)
      }
    }
    return dispatch(0)
  }
}

class LikeKoa {
  constructor() {
    this.middlewareList = []
  }

  use(fn) {
    this.middlewareList.push(fn)
    return this  // 链式调用
  }

  listen(...args) {
    const server = http.createServer(this.callback())
    server.listen(...args)
  }

  callback() {
    const fn = compose(this.middlewareList)

    return (req, res) => {
      const ctx = this.createContext(req, res)
      return this.handleRequest(ctx, fn)
    }
  }

  // 拼接ctx
  createContext(req, res) {
    const ctx = {
      req, 
      res
    }
    ctx.query = req.query
    return ctx
  }

  handleRequest(ctx, fn) {
    return fn(ctx)
  }
}

module.exports = LikeKoa
```
##### 测试代码
仿照官网示例demo实现的lib/test-koa.js，修改koa引用like-koa，ctx涉及method、url、body、set、get等处
```JavaScript
// lib/test-like-koa.js
const Koa = require('./like-koa'); // 替换node_modules/koa
const app = new Koa();

// logger
app.use(async (ctx, next) => {
  console.log('onion-1 ', 'start')
  await next();
  const rt = ctx['X-Response-Time'];  // 替换ctx.get
  console.log(`${ctx.req.method} ${ctx.req.url} - ${rt}`);  // 替换ctx.method/url
  console.log('onion-1 ', 'end')
});

// x-response-time
app.use(async (ctx, next) => {
  console.log('onion-2 ', 'start')
  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  ctx['X-Response-Time'] = `${ms}ms`;  // 替换ctx.set
  console.log('onion-2 ', 'end')
});

// response
app.use(async ctx => {
  console.log('onion-3 ', 'start')
  ctx.res.end('Hello from like-koa!');  // 替换ctx.body
  console.log('onion-3 ', 'end')
});

app.listen(3000);
```

### 上线与配置
服务器稳定性、充分利用服务器资源，以便提高性能、线上日志记录

#### PM2
Node.js Production Process Manager with a built-in Load Balancer.
pm2核心价值: 进程守护，系统崩溃自动重启； 启动多进程，充分利用CPU和内存；自带日志记录功能

##### 下载、安装
npm i pm2 -g    如果失败，可指定版本，如pm2@3.2.3
pm2 --version  查看版本

```JavaScript
// package.json  pm启动production环境
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "dev": "cross-env NODE_ENV=dev nodemon app.js",
  "prd": "cross-env NODE_ENV=production pm2 start ./bin/www"
},
```
运行命令npm run prd

##### 常用命令
pm2 start ... 启动
pm2 list 进程列表
pm2 start App name/id  重启
pm2 stop App name/id  停止
pm2 delete App name/id  删除
pm2 info App name/id  查看信息
pm2 log App name/id  查看日志
pm2 monit App name/id  监控CPU/内存

##### 进程守护
使用node app.js 或 nodemon app.js，进程崩溃则不能访问，使用pm2 系统崩溃可自动重启

如下，
```JavaScript
// test/pm2/app.js
const http = require('http')

const server = http.createServer((req, res) => {
  // 模拟错误
  if(req.url === '/err') {
    throw new Error('ooops !')
  }

  res.setHeader("Content-Type", "application/json")
  res.end(
    JSON.stringify({
      code: 2000,
      data: [2, 3, 5]
    })
  )
})

server.listen(3000, () => {
  console.log('listening at port 3000')
})
```
npm run dev, 以node或 nodemon启动项目，则直接崩溃，不会回复。
npm run prd 启动项目，当访问 http://localhost:3000/err，进程崩溃，但当访问http://localhost:3000 时，仍然可以访问，pm2 list 中restart此时增加了。
 
##### 配置
新建pm2 配置文件 test/pm2/pm2.conf.json，包括进程数量，日志文件目录等配置信息
```JSON
// test/pm2/pm2.conf.json
{
  "apps": {
    "name": "pm2-test",
    "script": "app.js",
    "watch": true,
    "ignore_watch": [
      "node_modules",
      "logs"
    ],
    "instances": 4,  // 多进程
    "out_file": "logs/out.log",
    "error_file": "logs/err.log",
    "log_date_format": "YYYY-MM-DD HH:mm:ss"
  }
}

// package.json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "dev": "cross-env NODE_ENV=dev nodemon app.js",
  // "prd": "cross-env NODE_ENV=production pm2 start app.js"
  "prd": "cross-env NODE_ENV=production pm2 start pm2.conf.json"
}
```
##### 多进程
原因: 单个进程内存受限，操作系统会限制一个进程的最大可用内存，如nodejs在64位操作系统，最大可占用1.4GB。
缺陷: 因此只有一个进程，则无法充分利用多核CPU优势和机器全部内存
问题: 但多进程内存无法共享，可以通过redis实现数据共享
实现: 如上， pm2.conf.json配置文件中增加 "instances": 4,  即可实现多进程

##### 服务器运维
服务器运维，一般由专业的OP人员或团队来处理，中小型工期推荐使用云服务，如阿里云的node平台


### 总结
![总结](/images/webserver.jpg)