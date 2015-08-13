---
date: Wed, 15 Jul 2015 06:41:12 +0000
title: Golang中空字符表示
author: tomheng
layout: post
permalink: /archives/1956.html
views: 168
categories:
  - LAMP
tags:
  - Golang
---
空字符（Null character）又称结束符，缩写NUL，是一个数值为0的控制字符。在C语言中空字符用来表示字符串的结束。

在C语言中也可以直接插入空字符：

<div class="codecolorer-container c blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />
        </div>
      </td>
      
      <td>
        <div class="c codecolorer">
          <span class="co2">#include <stdio.h></span><br /> <span class="kw4">int</span> main<span class="br0">&#40;</span><span class="kw4">void</span><span class="br0">&#41;</span><br /> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <a href="http://www.opengroup.org/onlinepubs/009695399/functions/puts.html"><span class="kw3">puts</span></a><span class="br0">&#40;</span><span class="st0">"hello<span class="es5">\0</span>world"</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

但是在Go中，类似的代码是不行的：

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw1">package</span> main<br /> <span class="kw1">import</span> <span class="st0">"fmt"</span><br /> <span class="kw4">func</span> main<span class="sy1">(){</span><br /> &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"hello\0world"</span><span class="sy1">)</span><br /> <span class="sy1">}</span><br /> <span class="co1">//print: /tmp/g.go:4: non-octal character in escape sequence: w</span>
        </div>
      </td>
    </tr>
  </table>
</div>

查看文档：https://golang.org/ref/spec#String_literals

修改一下即可：

<div class="codecolorer-container go blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />
        </div>
      </td>
      
      <td>
        <div class="go codecolorer">
          <span class="kw1">package</span> main<br /> <span class="kw1">import</span> <span class="st0">"fmt"</span><br /> <span class="kw4">func</span> main<span class="sy1">(){</span><br /> &nbsp; &nbsp; fmt<span class="sy3">.</span>Println<span class="sy1">(</span><span class="st0">"hello<span class="es2">\000</span>world"</span><span class="sy1">)</span><br /> <span class="sy1">}</span>
        </div>
      </td>
    </tr>
  </table>
</div>
