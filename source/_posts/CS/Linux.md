---
title: Linux
comments: true
date: 2019-01-26 21:51:47
categories: CS
tags: ['CS']
---

// 查看端口占用
lsof -i:443

// -t (tcp), -u (udp), -n 拒绝显示别名，能显示数字的全部转化为数字, -l 仅列出在Listen(监听)的服务状态, -p 显示建立相关链接的程序名
netstat -tunlp | grep 8000


// chomd 文件权限
// 分别表示User、Group、及Other的权限, r=4，w=2，x=1
sudo chmod 761 file



### linux 安装node 
```
查看 ls -la

下载安装包到服务器， 如用 wget (或者ssh/ftp)
wget https://nodejs.org/dist/v10.14.1/node-v10.14.1-linux-x64.tar.xz 

解压xz文件
xz -d node-v10.14.1-linux-x64.tar.xz  (解压缩，然后压缩包消失)
解压tar文件
tar -xvf node-v10.14.1-linux-x64.tar

创建软连接 ln -s source object 
ln -s /node-v10.14.1-linux-x64/bin/node  /usr/bin/node
ln -s /node-v10.14.1-linux-x64/bin/npm  /usr/bin/npm

删除软链接
rm -rf /usr/bin/node   (不是node/)

检验查看版本
node -v
npm -v

```