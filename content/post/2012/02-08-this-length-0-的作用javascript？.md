---
date: Wed, 08 Feb 2012 12:30:57 +0000
title: 'this.length >>> 0 的作用(Javascript)？'
author: tomheng
layout: post
permalink: /archives/1398.html
views: 911
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - JavaScript
  - seajs
---
在Javascript代码有时候会看到this.length >>> 0 这样的类似代码，那么this.length >>> 0这样的代码有什么用呢？  
要弄明白this.length >>> 0的作用，关键是要搞清楚 >>> 这个运算符是干什么的？

>>>在Javascript中代表无符号右移元算符，详细说明见：[ECMAScript 位运算符][1]。

在[Github][2]问了 **[lifesinger][3]**给出了一个this.length >>> 0 的作用更简易的总结**：**

  * 所有非数值转换成0
  * 所有大于等于 0 数取整数部分

<span style="color: #ff0000;">update:2012-04-24</span>

移位运算分为左移和右移，其中左移运算都是丢弃最高位，在右端补零。而右移预算则分为逻辑右移和算术右移动，逻辑右移在左端补零，算术右移则在左端扑最高有效位的值。

比如：x = 101101

x逻辑右移2位：001011

x算术右移2位：111011

javascript在这里的无符号右移即逻辑右移动,这个参照了JAVA中关于右移预算的规范。

 [1]: http://www.w3school.com.cn/js/pro_js_operators_bitwise.asp
 [2]: https://github.com
 [3]: https://github.com/lifesinger
