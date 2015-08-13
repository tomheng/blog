---
date: Tue, 25 Dec 2012 14:21:52 +0000
title: PHP中feof函数有可能造成死循环
author: tomheng
layout: post
permalink: /archives/1630.html
views: 214
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - PHP
---
前段时间公司的一个页面经常报告超时，本来以为是发邮件导致的超时。后来把邮件发送拆成异步之后，超时的问题并没有解决。于是仔细研究了下超时附近的代码段。

发现有类似一下的代码：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="kw2"><?php</span><br /> <br /> <span class="re0">$file</span> <span class="sy0">=</span> <a href="http://www.php.net/fopen"><span class="kw3">fopen</span></a><span class="br0">&#40;</span><span class="st0">"contacts.csv"</span><span class="sy0">,</span><span class="st0">"r"</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <span class="kw1">while</span><span class="br0">&#40;</span><span class="sy0">!</span> <a href="http://www.php.net/feof"><span class="kw3">feof</span></a><span class="br0">&#40;</span><span class="re0">$file</span><span class="br0">&#41;</span><span class="br0">&#41;</span><br /> <span class="br0">&#123;</span><br /> &nbsp; <a href="http://www.php.net/print_r"><span class="kw3">print_r</span></a><span class="br0">&#40;</span><a href="http://www.php.net/fgetcsv"><span class="kw3">fgetcsv</span></a><span class="br0">&#40;</span><span class="re0">$file</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <br /> <a href="http://www.php.net/fclose"><span class="kw3">fclose</span></a><span class="br0">&#40;</span><span class="re0">$file</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <br /> <span class="sy1">?></span>
        </div>
      </td>
    </tr>
  </table>
</div>

其实上面的代码就是W3scholl的[示例代码][1]。

如果没有仔细看过PHP的手册，大概看不出这段代码有什么问题？

问题的关键就在feof这个函数，手册上有明确的说明：

> If the passed file pointer is not valid you may get an infinite loop, because feof() fails to return TRUE.

详见：<http://cn2.php.net/feof>

所以上面的代码如果打开的文件失败，就会导致死循环，从而引起页面超时。

 [1]: http://www.w3school.com.cn/php/func_filesystem_fgetcsv.asp
