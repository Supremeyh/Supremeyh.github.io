---
title: vue常踩的坑 Trampling Pits
date: 2019-01-23 11:15:04
categories: Vue
tags: ['Vue', '踩坑']
---
### 小程序
* import 不能使用绝对路径，只能使用相对路径。但components内可以。如import { config } from '../config.js'
* thirdScriptError: code.startsWith is not a function，(定位代码，查看文档是否支持ES6)，说明code不是字符串，先用toString()处理下。如果是报undefied，说明code不存在。


* 不要在选项属性或回调上使用箭头函数，比如 created: () => console.log(this.a) 或 vm.$watch('a', newValue => this.myMethod())。因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例，经常导致 Uncaught TypeError: Cannot read property of undefined 或 Uncaught TypeError: this.myMethod is not a function 之类的错误。 

### tinymce
* tinymce解决上传的图片是相对路径问题
由于编辑的文章，可能会出现在app或者小程序中，这时候要求富文本中的图片是绝对路径，而不是相对路径。需要设置：
relative_urls : false,
remove_script_host : false