---
date: Sun, 17 Jul 2011 14:56:55 +0000
title: Javascript 中怎样用变量调用函数（想PHP中那样）
author: tomheng
layout: post
permalink: /archives/1151.html
views: 959
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - JavaScript
---
在PHP中可以用变量调用函数，比如：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="kw2"><?php</span><br /> &nbsp; &nbsp;<span class="kw2">function</span> say<span class="br0">&#40;</span><span class="re0">$words</span><span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp;<span class="kw1">echo</span> <span class="re0">$words</span><span class="sy0">;</span><br /> &nbsp; <span class="br0">&#125;</span><br /> &nbsp; <span class="re0">$f</span> <span class="sy0">=</span> <span class="st_h">'say'</span><span class="sy0">;</span><br /> &nbsp; <span class="re0">$f</span><span class="br0">&#40;</span><span class="st_h">'hello'</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="sy1">?></span>
        </div>
      </td>
    </tr>
  </table>
</div>

今天在测试Javascript程序的时候，想起来JS中能不能也像PHP中那样用变量来调用函数呢？

<div class="codecolorer-container javascript blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />
        </div>
      </td>
      
      <td>
        <div class="javascript codecolorer">
          <span class="kw2">function</span> say<span class="br0">&#40;</span>w<span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; <span class="kw3">alert</span><span class="br0">&#40;</span>w<span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="kw2">var</span> f <span class="sy0">=</span> <span class="st0">'say'</span><span class="sy0">;</span><br /> f<span class="br0">&#40;</span><span class="st0">'hello'</span><span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

这样做显然是行不通的，程序提示f函数不存在。

后来想到了可以借用eval函数来实现，修改程序：

<div class="codecolorer-container javascript blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />
        </div>
      </td>
      
      <td>
        <div class="javascript codecolorer">
          <span class="kw2">function</span> say<span class="br0">&#40;</span>w<span class="br0">&#41;</span><span class="br0">&#123;</span><br /> <span class="kw3">alert</span><span class="br0">&#40;</span>w<span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="kw2">var</span> f <span class="sy0">=</span> <span class="st0">'say'</span><span class="sy0">;</span><br /> <span class="kw1">eval</span><span class="br0">&#40;</span>f<span class="br0">&#41;</span><span class="br0">&#40;</span><span class="st0">'hello'</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="co1">//或者:eval(f+'("hello")');</span>
        </div>
      </td>
    </tr>
  </table>
</div>

这次程序按照预期的运行了， 其实这都是eval函数的功劳，到这里应该想起PHP中也有eval函数，他们的作用类似，都可以在自己的环境中把字符串当作代码运行出来，所以就有了上面的效果。  
最后再放出网上找到的一个例子代码，但是这个例子和我要表达的不是一个意思，可得看仔细了。

<div class="codecolorer-container javascript blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />
        </div>
      </td>
      
      <td>
        <div class="javascript codecolorer">
          <span class="kw2">function</span> say<span class="br0">&#40;</span>w<span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; &nbsp;<span class="kw3">alert</span><span class="br0">&#40;</span>w<span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="br0">&#40;</span><span class="kw2">function</span> a<span class="br0">&#40;</span>b<span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; b<span class="br0">&#40;</span><span class="st0">'hello'</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span>say<span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>
