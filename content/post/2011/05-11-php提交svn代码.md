---
date: Wed, 11 May 2011 14:16:59 +0000
title: PHP提交SVN代码
author: tomheng
layout: post
permalink: /archives/1033.html
views: 810
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - PHP
  - svn
---
代码是从[崔凯同学][1]那里抄来的。之所以要拿过来一是因为这种想法很好，我自己在工作中也是这样，经常把PHP当做一个小工具来实现，如果PHP够好的话，还可以在linux底下辅助shell完成一些功能。二是因为代码写的很棒，我这个专门学习PHP的都要惭愧的低下头喽！

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="kw2"><?php</span><br /> <a href="http://www.php.net/ob_start"><span class="kw3">ob_start</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">if</span> <span class="br0">&#40;</span><a href="http://www.php.net/isset"><span class="kw3">isset</span></a><span class="br0">&#40;</span><span class="re0">$_GET</span><span class="br0">&#91;</span><span class="st_h">'cleanup'</span><span class="br0">&#93;</span><span class="br0">&#41;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <span class="re0">$cmd</span> <span class="sy0">=</span> <span class="st_h">'svn cleanup /data/html/uicss.cn'</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span> <span class="kw1">else</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <span class="re0">$cmd</span> <span class="sy0">=</span> <span class="st_h">'svn update /data/html/uicss.cn --username cuikai --password 111222333'</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="re0">$resultado</span> <span class="sy0">=</span> <a href="http://www.php.net/join"><span class="kw3">join</span></a><span class="br0">&#40;</span><span class="st0">"<br>"</span><span class="sy0">,</span> executa<span class="br0">&#40;</span><span class="re0">$cmd</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="kw1">echo</span> <span class="re0">$resultado</span> <span class="sy0">.</span> <span class="st_h">'<br>'</span><span class="sy0">;</span><br /> <span class="kw2">function</span> executa<span class="br0">&#40;</span><span class="re0">$cmd</span><span class="sy0">,</span> <span class="re0">$pathInicial</span><span class="sy0">=</span><span class="kw4">null</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; <span class="re0">$resultado</span> <span class="sy0">=</span> <a href="http://www.php.net/array"><span class="kw3">array</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="re0">$handle</span> <span class="sy0">=</span> <a href="http://www.php.net/popen"><span class="kw3">popen</span></a><span class="br0">&#40;</span><span class="st0">"<span class="es4">$cmd</span> 2>&1"</span><span class="sy0">,</span> <span class="st_h">'r'</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="kw1">while</span> <span class="br0">&#40;</span><span class="re0">$read</span> <span class="sy0">=</span> <a href="http://www.php.net/fread"><span class="kw3">fread</span></a><span class="br0">&#40;</span><span class="re0">$handle</span><span class="sy0">,</span> <span class="nu0">20096</span><span class="br0">&#41;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="re0">$resultado</span><span class="br0">&#91;</span><span class="br0">&#93;</span> <span class="sy0">=</span> <span class="re0">$read</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="br0">&#125;</span><br /> &nbsp; &nbsp; <a href="http://www.php.net/pclose"><span class="kw3">pclose</span></a><span class="br0">&#40;</span><span class="re0">$handle</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="kw1">return</span> <span class="re0">$resultado</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> <span class="sy1">?></span>
        </div>
      </td>
    </tr>
  </table>
</div>

 [1]: http://uicss.cn/working-efficiency/
