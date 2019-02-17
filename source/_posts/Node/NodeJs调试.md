---
title: NodeJs调试
comments: true
date: 2019-02-17 00:21:51
categories: Node
tags: ['Node', 'debug']
---
* Inspect
优势：可查看上下文变量、观察当前函数调用堆栈、无入侵代码、可暂停执行指定代码
构成及原理：websockets服务监听命令、inspector协议、http服务元信息
执行 node --inspect index.js
控制台打印出 Debugger listening on ws://127.0.0.1:9229/ac0fc93c-40c7-45df-bff4-21048c1b7483

* Chrome DevTools
访问 chrome://inspect，点击配置按钮，确保Host和Post对应。一般为localhost:9229
访问元信息中的 devtoolsFrontendUrl, http://127.0.0.1:9229/json
点击浏览器控制台Node.js的DevTools绿色小图标


* Vscode
启动 fn + F5快捷键 开始调试
配置launch.json开启debug模式
打开自动附加模式Debug: toggole auto attach，然后命令行输入 node --inspect index.js 激活inspect


