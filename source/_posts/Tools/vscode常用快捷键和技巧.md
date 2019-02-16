---
title: vscode常用快捷键和技巧
comments: true
date: 2019-02-16 14:27:01
categories: Tools
tags: ['Tools', 'vscode']
---

* 命令行里启动vscode
在mac系统中，利用软连接命令ln -s，给可执行文件创造一个替身。在bin下面建立一个软连接。
```
cd /usr/local/bin/
ln -s "/Applications/Visual Studio Code.app/Contents/MacOS/Electron" vscode
```
即可通过 vscode /Users/sea/Desktop/OnGoing/HelloWorld/FullStack/Hexo 启动vscode，并打开Hexo目录。