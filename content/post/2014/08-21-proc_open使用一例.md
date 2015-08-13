---
date: Thu, 21 Aug 2014 05:33:53 +0000
title: proc_open使用一例
author: tomheng
layout: post
permalink: /archives/1834.html
views: 126
categories:
  - LAMP
tags:
  - MySQL
  - PHP基础
  - shell
  - 小工具
---
平时工作中很少用到[proc_open][1]这个函数，最近用这个函数做了一个数据库管理小程序。

实现如下：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="co1">//file name db.php</span><br /> <span class="re0">$mysql_list</span> <span class="sy0">=</span> <a href="http://www.php.net/array"><span class="kw3">array</span></a><span class="br0">&#40;</span><br /> &nbsp; &nbsp; <span class="st_h">'test'</span> <span class="sy0">=></span> <span class="st_h">'mysql -hlocalhost -utest -p123456 -P3306 test'</span><span class="sy0">,</span><br /> &nbsp; &nbsp; <span class="st_h">'test2'</span> <span class="sy0">=></span> <span class="st_h">'mysql -h172.16.190.199 -utest2 -p654321 -P3306 test2'</span><span class="sy0">,</span><br /> <span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="re0">$desc</span> <span class="sy0">=</span> <a href="http://www.php.net/array"><span class="kw3">array</span></a><span class="br0">&#40;</span><br /> &nbsp; &nbsp; <span class="nu0"></span> <span class="sy0">=></span> STDIN<span class="sy0">,</span><br /> &nbsp; &nbsp; <span class="nu0">1</span> <span class="sy0">=></span> STDOUT<span class="sy0">,</span><br /> &nbsp; &nbsp; <span class="nu0">2</span> <span class="sy0">=></span> STDERR<span class="sy0">,</span><br /> <span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="re0">$flag</span> <span class="sy0">=</span> <span class="sy0">@</span><span class="re0">$argv</span><span class="br0">&#91;</span><span class="nu0">1</span><span class="br0">&#93;</span><span class="sy0">;</span><br /> <span class="re0">$env</span> <span class="sy0">=</span> <span class="re0">$_ENV</span><span class="sy0">;</span><br /> <span class="re0">$cwd</span> <span class="sy0">=</span> <a href="http://www.php.net/getcwd"><span class="kw3">getcwd</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">if</span><span class="br0">&#40;</span><span class="sy0">!</span><a href="http://www.php.net/isset"><span class="kw3">isset</span></a><span class="br0">&#40;</span><span class="re0">$mysql_list</span><span class="br0">&#91;</span><span class="re0">$flag</span><span class="br0">&#93;</span><span class="br0">&#41;</span><span class="br0">&#41;</span><br /> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <a href="http://www.php.net/exit"><span class="kw3">exit</span></a><span class="br0">&#40;</span><span class="st0">"can not found the <span class="es4">$flag</span>"</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="re0">$re</span> <span class="sy0">=</span> <a href="http://www.php.net/proc_open"><span class="kw3">proc_open</span></a><span class="br0">&#40;</span><span class="re0">$cmd</span><span class="sy0">,</span> <span class="re0">$desc</span><span class="sy0">,</span> <span class="re0">$pipes</span><span class="sy0">,</span> <span class="re0">$cwd</span><span class="sy0">,</span> <span class="re0">$env</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">if</span><span class="br0">&#40;</span><span class="sy0">!</span><span class="re0">$re</span><span class="br0">&#41;</span><br /> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <a href="http://www.php.net/exit"><span class="kw3">exit</span></a><span class="br0">&#40;</span><span class="st_h">'failed to execute cmd'</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

这样有很多数据库需要管理的时候就比较容易了，直接配置下连接参数，然后：

php db.php test

简单实用吧~

补一个bash实现的。

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="co0">#!/bin/bash</span><br /> <span class="re2">flag</span>=<span class="re4">$1</span>;<br /> <span class="kw1">case</span> <span class="re1">$flag</span> <span class="kw1">in</span><br /> &nbsp; <span class="st_h">'test'</span><span class="br0">&#41;</span><br /> &nbsp; &nbsp; &nbsp; <span class="br0">&#40;</span>mysql <span class="re5">-hlocalhost</span> <span class="re5">-utest</span> <span class="re5">-p123456</span> <span class="re5">-P3306</span> <span class="kw3">test</span><span class="br0">&#41;</span>;<br /> &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br /> &nbsp; <span class="st_h">'test2'</span><span class="br0">&#41;</span><br /> &nbsp; &nbsp; &nbsp; <span class="br0">&#40;</span>mysql -h172.16.190.199 <span class="re5">-utest2</span> <span class="re5">-p654321</span> <span class="re5">-P3306</span> test2<span class="br0">&#41;</span>;<br /> &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br /> &nbsp; &nbsp;<span class="sy0">*</span><span class="br0">&#41;</span><br /> &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st_h">'can not found the flag cmd'</span>;<br /> &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br /> <span class="kw1">esac</span>;
        </div>
      </td>
    </tr>
  </table>
</div>

 [1]: http://cn2.php.net/proc_open
