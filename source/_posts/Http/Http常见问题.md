---
title: Http常见问题
comments: true
date: 2019-02-13 09:47:59
categories: Http
tags: ['Http', '常见问题']
---

* Response中set-cookie里的值不能写入浏览器cookie
原因是因为响应头中的 cookie 是带有 domain 属性的（domain=.test.com），而从 Request URL 中可以看到，我们发起请求的域名是 localhost，请求和响应的 domain 不匹配，浏览器就帮你自动忽略了。
修改服务器的配置，把响应中的 domain 去掉, cookie 就写进去了。




