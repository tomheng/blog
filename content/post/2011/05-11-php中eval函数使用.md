---
date: Wed, 11 May 2011 13:53:21 +0000
title: php中eval函数使用
author: tomheng
layout: post
permalink: /archives/1031.html
views: 538
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - PHP
---
eval 每次都让我联想到evil，所以我很少使用这个函数，不过有时候使用一下，还是能够带来不少的方便的。比如下面的代码：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="re0">$a</span> <span class="sy0">=</span> <span class="st_h">'1,2,4,5,6'</span><span class="sy0">;</span><br /> <a href="http://www.php.net/eval"><span class="kw3">eval</span></a><span class="br0">&#40;</span><span class="st0">"<span class="es1">\$</span>b=array(<span class="es4">$a</span>);"</span><span class="br0">&#41;</span><span class="sy0">;</span><span class="co1">//引号中有一个分号的，不要丢了。</span><br /> <a href="http://www.php.net/print_r"><span class="kw3">print_r</span></a><span class="br0">&#40;</span><span class="re0">$b</span><span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

运行一下就知道效果了。
