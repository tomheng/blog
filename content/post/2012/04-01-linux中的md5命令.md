---
date: Sun, 01 Apr 2012 05:20:25 +0000
title: Linux中的md5命令
author: tomheng
layout: post
permalink: /archives/1477.html
views: 2043
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - shell
---
今天想生成一个字符串的MD5, shell中有一个md5sum命令，但是这个适合用来验证MD5,如果要生成字符串的MD5可以这样

echo -n &#8216;123456&#8217; | md5sum

这样不是很方便，so我写了个超简单的shell脚本

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="co0">#!/bin/bash</span><br /> <span class="kw3">echo</span> <span class="re5">-n</span> <span class="re4">$1</span> <span class="sy0">|</span> md5sum <span class="sy0">|</span> <span class="kw2">awk</span> <span class="st_h">'{print $1}'</span>
        </div>
      </td>
    </tr>
  </table>
</div>

<https://gist.github.com/2271597>

将上面的代码写入/user/bin/md5文件，并且给文件加上执行权限。

于是世界变得美好了；

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
          md5 <span class="nu0">123456</span><br /> <br /> <span class="co0">#print e10adc3949ba59abbe56e057f20f883e</span>
        </div>
      </td>
    </tr>
  </table>
</div>
