---
date: Mon, 10 Jan 2011 12:38:58 +0000
title: strlen()与mb_strlen的区别是什么？
author: tomheng
layout: post
permalink: /archives/936.html
robotsmeta:
  - index,follow
views: 968
categories:
  - LAMP
tags:
  - PHP
---
中文的字符编码有很多，不同字符编码下，一个中文字符占的字节数是不同的。如果是UTF-8编码，那么中文字符的长度是3个字节，ANSI编码是2个字节。strlen()和mb\_strlen()这两个函数是用来获得字符串长度的，区别在于mb\_strlen()可以把多字节字符区别出来，把多字节字符当做一个字符来处理。

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
          <span class="kw2"><?php</span> <br /> <span class="re0">$str</span><span class="sy0">=</span><span class="st0">"北京"</span><span class="sy0">;</span><br /> <span class="kw1">echo</span> <a href="http://www.php.net/strlen"><span class="kw3">strlen</span></a><span class="br0">&#40;</span><span class="re0">$str</span><span class="br0">&#41;</span><span class="sy0">.</span><span class="st0">""</span><span class="sy0">.</span><a href="http://www.php.net/mb_strlen"><span class="kw3">mb_strlen</span></a><span class="br0">&#40;</span><span class="re0">$str</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="co1">//output</span><br /> <span class="co1">//6</span><br /> <span class="co1">//2</span><br /> <span class="sy1">?></span>
        </div>
      </td>
    </tr>
  </table>
</div>

因为在utf中一个中文字符是三个字节，所以“北京”这个字符串的长度是6，而mb_strlen()处理正确是两个字符。  
其实strlen是按英文来的，英文字母在任何编码中都是一个字节。所以strlen在含有其他语言文字的时候，计算的其实可以理解为字节数目。  
而mb_strlen计算的是字符数目。
