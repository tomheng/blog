---
date: Sat, 05 May 2012 01:01:16 +0000
title: ubuntu 运行apt-get 时遇到“ Hash Sum mismatch ”
author: tomheng
layout: post
permalink: /archives/1520.html
views: 3081
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - Ubuntu
---
今天升级ubuntu到12.04的时候，运行apt-get update遇到了“Hash Sum mismatch”错误，通过一下方法得到了解决：

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
          <span class="sy0">//</span>先运行<br /> <span class="kw2">sudo</span> <span class="kw2">apt-get clean</span><br /> <span class="sy0">//</span>再运行<br /> <span class="kw2">sudo</span> <span class="kw2">apt-get update</span> <span class="re5">--fix-missing</span>
        </div>
      </td>
    </tr>
  </table>
</div>
