---
date: Mon, 09 Apr 2012 03:27:22 +0000
title: 如何利用shell命令在特定文件类型中做搜索
author: tomheng
layout: post
permalink: /archives/1488.html
views: 288
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - shell
---
比如在log文件中搜索相应的代码：

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="co0">#第一种方法,{}在这里指find搜索的结果</span><br /> <span class="kw2">find</span> <span class="re1">$dir</span> <span class="re5">-type</span> f <span class="re5">-name</span> <span class="st_h">'*.log'</span> <span class="re5">-exec</span> <span class="kw2">grep</span> <span class="re5">-o</span> <span class="re1">$pattern</span> <span class="br0">&#123;</span><span class="br0">&#125;</span> \;<br /> <span class="co0">#第二种方法</span><br /> <span class="kw2">find</span> <span class="re1">$dir</span> <span class="re5">-type</span> f <span class="re5">-name</span> <span class="st_h">'*.log'</span> <span class="re5">-print0</span> <span class="sy0">|</span> <span class="kw2">xargs</span> <span class="re5">-0</span> <span class="kw2">grep</span> <span class="re5">-o</span> <span class="re1">$pattern</span>
        </div>
      </td>
    </tr>
  </table>
</div>

参考：<http://zh.wikipedia.org/wiki/Xargs>  
摘录自：<http://www.unix.com/unix-dummies-questions-answers/53172-how-do-i-grep-specific-file-types.html>
