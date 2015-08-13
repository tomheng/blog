---
date: Sat, 06 Aug 2011 15:43:59 +0000
title: 是不是要用array_key_exists函数
author: tomheng
layout: post
permalink: /archives/1160.html
views: 578
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - PHP基础
---
在PHP中判断一个数组的特定索引的值是否存在，通常会用array\_key\_exists()函数。但是我们可能更熟悉另一个判断变量是否存在的函数isset()，当然我们也可以用isset()来判断数组中的值是否设置。

比如：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="re0">$a</span> <span class="sy0">=</span> <a href="http://www.php.net/array"><span class="kw3">array</span></a><span class="br0">&#40;</span><span class="st_h">'site'</span><span class="sy0">=></span><span class="st_h">'blog.webfuns.net'</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <a href="http://www.php.net/isset"><span class="kw3">isset</span></a><span class="br0">&#40;</span><span class="re0">$a</span><span class="br0">&#91;</span><span class="st_h">'site'</span><span class="br0">&#93;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="co1">//同：array_key_exists('site', $a);</span>
        </div>
      </td>
    </tr>
  </table>
</div>

两种方法都能达到同样的目的，但是通过测试发现isset()函数的效率更高。

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="re0">$a</span> <span class="sy0">=</span> <a href="http://www.php.net/range"><span class="kw3">range</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="sy0">,</span><span class="nu0">1000</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="re0">$a</span><span class="br0">&#91;</span><span class="st_h">'tom'</span><span class="br0">&#93;</span> <span class="sy0">=</span> <span class="st_h">'heng'</span><span class="sy0">;</span><br /> <span class="re0">$begin</span> <span class="sy0">=</span> <a href="http://www.php.net/microtime"><span class="kw3">microtime</span></a><span class="br0">&#40;</span><span class="kw4">true</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">for</span><span class="br0">&#40;</span><span class="re0">$i</span><span class="sy0">=</span><span class="nu0"></span><span class="sy0">;</span> <span class="re0">$i</span><span class="sy0"><</span><span class="nu0">100000</span><span class="sy0">;</span> <span class="re0">$i</span><span class="sy0">++</span><span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; &nbsp; <a href="http://www.php.net/array_key_exists"><span class="kw3">array_key_exists</span></a><span class="br0">&#40;</span><span class="st_h">'tom'</span><span class="sy0">,</span> <span class="re0">$a</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="re0">$end</span> <span class="sy0">=</span> <a href="http://www.php.net/microtime"><span class="kw3">microtime</span></a><span class="br0">&#40;</span><span class="kw4">true</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">for</span><span class="br0">&#40;</span><span class="re0">$i</span><span class="sy0">=</span><span class="nu0"></span><span class="sy0">;</span> <span class="re0">$i</span><span class="sy0"><</span><span class="nu0">100000</span><span class="sy0">;</span> <span class="re0">$i</span><span class="sy0">++</span><span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; &nbsp; <a href="http://www.php.net/isset"><span class="kw3">isset</span></a><span class="br0">&#40;</span><span class="re0">$a</span><span class="br0">&#91;</span><span class="st_h">'tom'</span><span class="br0">&#93;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="re0">$end2</span> <span class="sy0">=</span> <a href="http://www.php.net/microtime"><span class="kw3">microtime</span></a><span class="br0">&#40;</span><span class="kw4">true</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">echo</span> <span class="re0">$end</span> <span class="sy0">-</span> <span class="re0">$begin</span><span class="sy0">;</span><br /> <span class="kw1">echo</span> <span class="st_h">'<br/>'</span><span class="sy0">;</span><br /> <span class="kw1">echo</span> <span class="re0">$end2</span> <span class="sy0">-</span> <span class="re0">$end</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

测试结果：

0.053144931793213  
0.014554023742676

可以看到isset()函数的效率要高四倍左右，这下知道以后该怎么办了吧。
