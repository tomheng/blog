---
date: Wed, 12 Oct 2011 05:48:42 +0000
title: mysql 跨库的触发器
author: tomheng
layout: post
permalink: /archives/1169.html
views: 929
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - MySQL
---
<div id="_mcePaste">
  今天遇到一个问题，想通过写一个垮库的触发器来解决。
</div>

<div>
  其实垮库的触发器和同一个库的触发器，没有多大的区别。
</div>

<div>
  只需要在操作的表前面加上数据库就可以了。
</div>

<div>
  另外，触发器是存储在存在触发事件的表对应的数据库中的，
</div>

<div>
  但是在触发执行语句中不用显示的更换数据库(use　database)。
</div>

示例代码：

<div>
  <div class="codecolorer-container mysql blackboard" style="overflow:auto;white-space:nowrap;">
    <table cellspacing="0" cellpadding="0">
      <tr>
        <td class="line-numbers">
          <div>
            1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />
          </div>
        </td>
        
        <td>
          <div class="mysql codecolorer">
            <span class="co2">#示例1</span><br /> DELIMITER $<br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=USE"><span class="kw1">USE</span></a> <span class="st0">`tom`</span>$<br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=DROP"><span class="kw1">DROP</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=TRIGGER"><span class="kw1">TRIGGER</span></a> <span class="coMULTI">/*!50032 IF EXISTS */</span> <span class="st0">`trigger1`</span>$<br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=CREATE"><span class="kw1">CREATE</span></a><br /> <span class="coMULTI">/*!50017 DEFINER = 'root'@'localhost' */</span><br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=TRIGGER"><span class="kw1">TRIGGER</span></a> <span class="st0">`trigger1`</span> <a href="http://search.mysql.com/search?site=refman-%35%31&q=AFTER"><span class="kw1">AFTER</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=INSERT"><span class="kw2">INSERT</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=ON"><span class="kw1">ON</span></a> <span class="st0">`tom`</span>.<span class="st0">`stu`</span><br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=FOR%20EACH%20ROW"><span class="kw1">FOR EACH ROW</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=BEGIN"><span class="kw1">BEGIN</span></a><br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=INSERT"><span class="kw2">INSERT</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=INTO"><span class="kw1">INTO</span></a> <span class="st0">`test`</span>.<span class="st0">`coder`</span> <a href="http://search.mysql.com/search?site=refman-%35%31&q=VALUES"><span class="kw1">VALUES</span></a><span class="br0">&#40;</span>new.id<span class="sy2">,</span>new.name<span class="br0">&#41;</span><span class="sy2">;</span><br /> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/control-flow-functions.html"><span class="kw12">END</span></a><br /> $<br /> DELIMITER <span class="sy2">;</span><br /> <span class="co2">#示例2</span><br /> DELIMITER <span class="sy1">&</span> <a href="http://search.mysql.com/search?site=refman-%35%31&q=USE"><span class="kw1">USE</span></a> &nbsp;<span class="st0">`shine`</span> <span class="sy1">&</span> <br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=DROP"><span class="kw1">DROP</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=TRIGGER"><span class="kw1">TRIGGER</span></a> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/control-flow-functions.html"><span class="kw12">IF</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=EXISTS"><span class="kw1">EXISTS</span></a> &nbsp;<span class="st0">`object<span class="es1">_</span>permission<span class="es1">_</span>trigger`</span> <span class="sy1">&</span> <br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=CREATE"><span class="kw1">CREATE</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=TRIGGER"><span class="kw1">TRIGGER</span></a> <span class="st0">`object<span class="es1">_</span>permission<span class="es1">_</span>trigger`</span> <a href="http://search.mysql.com/search?site=refman-%35%31&q=AFTER"><span class="kw1">AFTER</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=UPDATE"><span class="kw1">UPDATE</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=ON"><span class="kw1">ON</span></a> &nbsp;<span class="st0">`object`</span><br /> &nbsp;<a href="http://search.mysql.com/search?site=refman-%35%31&q=FOR%20EACH%20ROW"><span class="kw1">FOR EACH ROW</span></a> <a href="http://search.mysql.com/search?site=refman-%35%31&q=BEGIN"><span class="kw1">BEGIN</span></a> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/control-flow-functions.html"><span class="kw12">IF</span></a> old.<a href="http://search.mysql.com/search?site=refman-%35%31&q=TYPE"><span class="kw1">type</span></a> <span class="sy1">=</span> &nbsp;<span class="st0">'product'</span> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/non-typed-operators.html"><span class="kw10">OR</span></a> old.<a href="http://search.mysql.com/search?site=refman-%35%31&q=TYPE"><span class="kw1">type</span></a> <span class="sy1">=</span> &nbsp;<span class="st0">'business'</span> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/control-flow-functions.html"><span class="kw12">THEN</span></a> <br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=UPDATE"><span class="kw1">UPDATE</span></a> &nbsp;<span class="st0">`permission`</span> <a href="http://search.mysql.com/search?site=refman-%35%31&q=SET"><span class="kw1">SET</span></a> &nbsp;<span class="st0">`name`</span> <span class="sy1">=</span> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/string-functions.html"><span class="kw14">REPLACE</span></a><span class="br0">&#40;</span> &nbsp;<span class="st0">`name`</span> <span class="sy2">,</span> old.name<span class="sy2">,</span> new.name <span class="br0">&#41;</span> <br /> <a href="http://search.mysql.com/search?site=refman-%35%31&q=WHERE"><span class="kw1">WHERE</span></a> <span class="st0">`object<span class="es1">_</span>id`</span> <span class="sy1">=</span> old.id<span class="sy2">;</span><br /> <br /> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/control-flow-functions.html"><span class="kw12">END</span></a> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/control-flow-functions.html"><span class="kw12">IF</span></a> <span class="sy2">;</span><br /> <br /> <a href="http://dev.mysql.com/doc/refman/%35%2E%31/en/control-flow-functions.html"><span class="kw12">END</span></a> <span class="sy1">&</span> <br /> DELIMITER<span class="sy2">;</span>
          </div>
        </td>
      </tr>
    </table>
  </div>
</div>

<div id="_mcePaste">
  关于<a href='http://dev.mysql.com/doc/refman/5.1/zh/triggers.html'>触发器</a>
</div>
