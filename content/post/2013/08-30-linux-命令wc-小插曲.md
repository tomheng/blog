---
date: Fri, 30 Aug 2013 14:11:09 +0000
title: Linux 命令wc 小插曲
author: tomheng
layout: post
permalink: /archives/1736.html
views: 110
categories:
  - LAMP
tags:
  - shell
  - wc
---
做开发的或多或少都会接触过wc 这个命令吧，手册里说这个命令的的作用是“print newline, word, and byte counts for each file”，一般情况下大家可以用wc -l $file 来统计文件中行数。

但是你如果按下面这样操作，就会发现一个有趣的事情。

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          php <span class="re5">-r</span> <span class="st_h">'file_put_contents("/tmp/a.txt", implode(PHP_EOL, array(1, 2)));'</span>;<br /> <span class="kw2">wc</span> <span class="re5">-l</span> <span class="sy0">/</span>tmp<span class="sy0">/</span>a.txt <span class="co0">#one line</span><br /> <span class="kw2">awk</span> <span class="st_h">'{print NR}'</span> <span class="sy0">/</span>tmp<span class="sy0">/</span>a.txt <span class="co0">#two lines</span>
        </div>
      </td>
    </tr>
  </table>
</div>

我理解的这个地方正常应该显示两行才对，因为换行符意思就是结束上一行开始新一行，所以在有一个换行符的情况下应该按两行来处理。这个在工作中有时可能需要注意下，有很多程序可能写入文件的时候都没有在最后添加换行符，所以如果直接用wc可能不够准确。
