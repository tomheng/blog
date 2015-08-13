---
date: Fri, 06 Aug 2010 13:01:30 +0000
title: JavaScript函数闭包的实例程序
author: tomheng
layout: post
permalink: /archives/743.html
robotsmeta:
  - index,follow
views: 826
categories:
  - WordPress
tags:
  - JavaScript
---
最近在学习JavaScript，一直感觉这个语言很诡异，和其他的语言有些不同。最近暑假期间补充知识，正好来学习一下。今天提供一个关于JavaScript中函数闭包的一个实例程序。先来解释下什么是函数闭包:

> **函数闭包**-即函数定义和函数表达式位于另一个函数的函数体内。而且，这些内部函数可以访问它们所在的外部函数中声明的所有局部变量、参数和声明的其他内部函数。当其中一个这样的内部函数在包含它们的外部函数之外被调用时，就会形成闭包。也就是说，内部函数会在外部函数返回后被执行。而当这个内部函数执行时，它仍然必需访问其外部函数的局部变量、参数以及其他内部函数。

<div>
  下面的两个实例程序的作用是给一组标签添加鼠标事件函数，当点击一个节点是，弹出一个对话框显示节点的序号。
</div>

<div>
  <strong>实例一</strong>：未利用函数闭包错误的实例
</div>

<div>
  <div>
    var add_the_handlers=function (nodes){
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span>var i;
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span>for(i=0;i<nodes.length;i++){
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span> nodes[i].onclick=function(e){
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span>alert(i);<span style="white-space: pre;"> </span>
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span> };
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span>}
  </div>
  
  <div id="_mcePaste">
    }
  </div>
  
  <div id="_mcePaste">
    add_the_handlers(document.getElementsByTagName(&#8220;div&#8221;));
  </div>
  
  <div>
    <strong>实例二：</strong>利用函数闭包正确的实例
  </div>
</div>

<div>
  <div>
    var add_the_handlers=function (nodes){
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span>var i;
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span>for(i=0;i<nodes.length;i++){
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span> nodes[i].onclick=<span style="color: #ff0000;">function(i){</span>
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"><span style="color: #ff0000;"> </span></span><span style="color: #ff0000;"> //alert(i);</span>
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"><span style="color: #ff0000;"> </span></span><span style="color: #ff0000;"> return function (e){</span>
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"><span style="color: #ff0000;"> </span></span><span style="color: #ff0000;"> alert(i);</span>
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"><span style="color: #ff0000;"> </span></span><span style="color: #ff0000;"> };</span>
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"><span style="color: #ff0000;"> </span></span><span style="color: #ff0000;"> }(i);</span>
  </div>
  
  <div id="_mcePaste">
    <span style="white-space: pre;"> </span>}
  </div>
  
  <div id="_mcePaste">
    }
  </div>
  
  <div id="_mcePaste">
    add_the_handlers(document.getElementsByTagName(&#8220;div&#8221;));
  </div>
  
  <div>
    <span style="color: #3366ff;">PS:学习笔记只用，不做深入解释（<span style="color: #808080;">功力不够，继续修炼</span>）。</span>
  </div>
</div>
