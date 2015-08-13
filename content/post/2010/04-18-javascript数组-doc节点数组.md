---
date: Sun, 18 Apr 2010 01:06:05 +0000
title: javascript数组-DOM节点数组
author: tomheng
layout: post
permalink: /archives/576.html
robotsmeta:
  - index,follow
views: 829
categories:
  - WordPress
tags:
  - JavaScript
---
最近发现JavaScript还是一门很有意思的语言，值得好好学习一下。前些天调试一个简单程序时，发现一个问题或者说自己犯了一个简单的错误。

**下面我举一个简单的例子说明一下遇到的问题：**

我有个HTML文档如下

<pre class="brush: html; first-line: 1"><table>
  
  
  
  <tr>
    <td>
      tomheng
    </td>
    
  </tr>
  
  
</table>


<table>
  
  
  
  <tr>
    <td>
      住在<a href="http://blog.webfuns.net/">webfuns 趣味互联网</a>
    </td>
    
  </tr>
  
  
</table>
</pre>

我想要删除里面的table节点元素，注意HTML文档中有两个以上的Table节点于是写了如下的代码。

<pre class="brush: javascript; first-line: 1">tb=document.getElementsByTagName("table");
  for(var i=0;i=tb.length;i++){
      document.body.removeChild(tb[i]);
  }</pre>

**问题分析**

<span style="color: #ff00ff;">有没有发现问题啊？上面的代码是没有错误的，但是就是不能实现我想要的效果，上面的代码不能删除最后一个元素。如果元素多了，你能想出他是怎样删除的吗？</span>

我们都知道数组的复制在大多数语言中都是引用传值的，在JavaScript中也不例外，而且数组的存储结构一般都是采用栈的形式，所以上面的问题就很好解释了。tb变量实质上得到的是DOM中的Table节点数组的引用，我们在For循环中删除Table节点元素同样也会反应在tb这个数组中，这样以来，每删除一个元素数组的长度就会发生变化，根据栈结构的特点，元素自动向上弹出，所以我们使用这样的删除方式只能删除索引为奇数的元素。既然知道的了原因，那么解决起来就比较简单了。

** 正确代码如下：**

<pre class="brush:javascript;firt-line:1">tb=document.getElementsByTagName("table");
  while(tb.length){
     document.body.removeChild(tb[0]);
 }
/*充分利用栈结构的特点，总是删除第一个元素，直到为空*/</pre>
