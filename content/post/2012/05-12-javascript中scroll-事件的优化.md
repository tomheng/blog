---
date: Sat, 12 May 2012 11:29:05 +0000
title: javascript中scroll 事件的优化
author: tomheng
layout: post
permalink: /archives/1525.html
views: 1166
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - JavaScript
---
最近再做项目时遇到的问题，如果给window绑定一个scoll事件，在chrome和firefox下都没有问题，但是在IE下面可能会比较影响效率，页面会比较卡。这是因为IE下面scroll事件触发的更频繁，如果在事件处理函数中做一些比较昂贵的处理（比如DOM的增删），那么就会影响页面的响应。不过我们可以通过一定的方法来优化scrooll事件，让他触发的不是那么频繁。

<div class="codecolorer-container javascript blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />
        </div>
      </td>
      
      <td>
        <div class="javascript codecolorer">
          <span class="kw2">var</span> timer <span class="sy0">=</span> <span class="nu0"></span><span class="sy0">,</span><br /> delay <span class="sy0">=</span> <span class="nu0">50</span><span class="sy0">;</span> <span class="co1">//这个值越小触发的越频繁</span><br /> <span class="kw2">var</span> handler <span class="sy0">=</span> <span class="kw2">function</span><span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> timer <span class="sy0">=</span> <span class="nu0"></span><span class="sy0">;</span><br /> <span class="co1">//scroll事件发生时要做的事情</span><br /> <span class="br0">&#125;</span><br /> <br /> $<span class="br0">&#40;</span>window<span class="br0">&#41;</span>.<span class="kw3">scroll</span><span class="br0">&#40;</span><span class="kw2">function</span><span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> <span class="kw1">if</span> <span class="br0">&#40;</span>timer<span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> clearTimeout<span class="br0">&#40;</span>timer<span class="br0">&#41;</span><span class="sy0">;</span><br /> timer <span class="sy0">=</span> <span class="nu0"></span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> timer <span class="sy0">=</span> setTimeout<span class="br0">&#40;</span>handler<span class="sy0">,</span> delay<span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

PS：不过在项目中最后发现其实不是scroll事件造成的页面卡，还在寻觅中···  
参考：[JQuery: My &#8216;scroll&#8217; event is CRAZY slow. What am I doing wrong?][1]

 [1]: http://stackoverflow.com/questions/2158268/jquery-my-scroll-event-is-crazy-slow-what-am-i-doing-wrong
