---
date: Fri, 12 Sep 2014 06:35:16 +0000
title: PHP 中mysql_ping 函数小坑
author: tomheng
layout: post
permalink: /archives/1850.html
views: 114
categories:
  - LAMP
tags:
  - PHP基础
---
官方文档是这样介绍的：

> mysql\_ping() 检查到服务器的连接是否正常。如果断开，则自动尝试连接。本函数可用于空闲很久的脚本来检查服务器是否关闭了连接，如果有必要则重新连接上。如果到服务器的连接可用则 mysql\_ping() 返回 TRUE，否则返回 FALSE。

看一眼可能会以为mysql_ping 就两个返回值（TRUE | FALSE),但是实际情况是还有第三个返回值。这个问题官方文档下方是有人评论的，而且是在七年前:(

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="co1">//When checking if a $resource works...</span><br /> <span class="co1">//be prepared that mysql_ping returns NULL as long as $resource is no correct mysql resource.</span><br /> <span class="re0">$resource</span> <span class="sy0">=</span><span class="kw4">NULL</span><span class="sy0">;</span><br /> <a href="http://www.php.net/var_dump"><span class="kw3">var_dump</span></a> <span class="sy0">=</span> <span class="sy0">@</span><a href="http://www.php.net/mysql_ping"><span class="kw3">mysql_ping</span></a><span class="br0">&#40;</span><span class="re0">$resource</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="co2"># showing NULL<br /> </span><span class="co1">//This could be used to decide of a current $resource is a mysql or a mysqli connection when //nothing else is available to do that...</span>
        </div>
      </td>
    </tr>
  </table>
</div>

越来越觉着PHP是个玩具型的东西。。。
