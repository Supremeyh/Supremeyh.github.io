---
title: white-space、word-wrap、word-break
comments: true
date: 2019-02-26 16:06:09
categories: CSS
tags: ['CSS']
---

* 浏览器默认情况
如果有一个单词很长，导致一行剩下的空间放不下，则浏览器会把这个单词挪到下一行去。但是如果新行还是放不下，则会溢出父元素

* CJK 
指中文/日文/韩文 统一表意文字


* white-space 控制空白字符的显示，同时还能控制是否自动换行
normal   合并连续的空白符，换行符会被当作空白符来处理，折行。 
nowrap   合并连续的空白符，文本会在在同一行上继续，不折行，直到遇到 br 标签为止。 
pre      连续的空白符会被保留，不折行。在遇到换行符或者 br 元素时才会换行。 类似pre标签。
pre-wrap 保留所有空白符、换行符，折行。
pre-line 合并连续的空白符，换行符保留，折行。

* word-wrap（overflow-wrap） 控制长单词或URL是否被拆分换行
normal      表示在正常的单词结束处换行。
break-word  如果行内没有多余的地方容纳该单词到结尾，则那些正常的不能被分割的单词会被强制分割换行。比较温柔，会先另起一行，新的行放不到再把单词断了。

* word-break 控制单词如何被拆分换行
normal    使用默认的断行规则。CJK文本会自动行，非CJK文本不会自动换行，有空格时，非CJK文本也会换行。
break-all 对于非CJK文本，可在任意字符间断行。所有单词碰到边界一律拆分换行。比较暴力，不会去新起一行，如果放不下，则会强制把单词折断。将非CJK内容作为CJK处理。
keep-all  对于CJK文本不断行，非CJK 文本表现同 normal。 可以理解为只有空格可以触发自动换行。将CJK内容当成非CJK处理


* 单行文本的溢出显示省略号
```CSS
.box{
  width: 100px;
  white-space: nowrap;
  text-overflow: ellipsis; 
  overflow: hidden;
}
```

* 多行文本溢出显示省略号  广泛使用但文字未超出行也会出现省略号
```CSS
.box{
  width: 100px;
  position: relative;
  overflow: hidden;
}
.box::after{
  position: absolute;
  content: '...';
  right: 0;
  bottom: 0;

  background:-webkit-linear-gradient(left,transparent,#fff 55%);
  background:-o-linear-gradient(right,transparent,#fff 55%);
  background:-moz-linear-gradient(right,transparent,#fff 55%);
  background:linear-gradient(to right,transparent,#fff 55%);
}
```

* 多行文本溢出显示省略号 仅适用webkit内核
```CSS
.box{
  width: 100px;
  display: -webkit-box;  // 将对象作为弹性伸缩盒子模型显示
  -webkit-box-orient: vertical;  // 设置或检索伸缩盒对象的子元素的排列方式 
  -webkit-line-clamp: 3;  // 显示的文本的行数
  white-space: nowrap;
  text-overflow: ellipsis; 
  overflow: hidden;
}
```