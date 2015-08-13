---
date: Wed, 25 Aug 2010 12:16:46 +0000
title: get_class()函数
author: tomheng
layout: post
permalink: /archives/782.html
robotsmeta:
  - index,follow
views: 902
categories:
  - LAMP
tags:
  - PHP
---
今天介绍个函数，举例示之。

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="kw2">class</span> A<span class="br0">&#123;</span><br /> <span class="kw2">function</span> __construct<span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#123;</span><br /> <span class="kw1">echo</span> <span class="st0">"My name is "</span> <span class="sy0">,</span> <a href="http://www.php.net/get_class"><span class="kw3">get_class</span></a><span class="br0">&#40;</span><span class="re0">$this</span><span class="br0">&#41;</span> <span class="sy0">,</span> <span class="st0">"<span class="es1">\n</span>"</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="br0">&#125;</span><br /> <span class="kw2">class</span> B <span class="kw2">extends</span> A<span class="br0">&#123;</span><br /> <span class="br0">&#125;</span><br /> <span class="re0">$b</span><span class="sy0">=</span><span class="kw2">new</span> B<span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

<div>
  <span style="color: #003300;">//输出结果：</span><span style="line-height: normal; font-size: small;"><span style="color: #003300;">My name is B</span></span>
</div>

<div>
  <span style="line-height: normal; font-size: small;"><span style="color: #ff0000;">get_class 用在A和用在B中是一样的。比较好玩的特性，在此记录一下，以备后查！</span></span>
</div>
