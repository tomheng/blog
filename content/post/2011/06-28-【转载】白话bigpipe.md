---
date: Tue, 28 Jun 2011 14:52:09 +0000
title: 【转载】白话BigPipe
author: tomheng
layout: post
permalink: /archives/1096.html
views: 513
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - bigpipe
---
所谓BigPipe，指的是<a href="http://www.facebook.com/notes/facebook-engineering/bigpipe-pipelining-web-pages-for-high-performance/389414033919" target="_blank">Facebook</a>开发的用来改善客户端响应速度的技术。本质上讲，其实它并不是新事物，原理上等同于Yahoo在<a href="http://developer.yahoo.com/performance/rules.html" target="_blank">Best Practices for Speeding Up Your Web Site</a>里提出的Flush the Buffer Early，不过BigPipe的实现更灵活，所以有必要了解一二。

我们平常浏览网页时的体验通常是串行的：浏览器发起请求，服务器收到后渲染页面，在此期间，浏览器除了等待别无选择，演示代码如下：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="kw2"><?php</span><br /> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="re0">$header</span> <span class="sy0">=</span> <span class="st_h">'header'</span><span class="sy0">;</span><br /> <br /> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="re0">$content</span> <span class="sy0">=</span> <span class="st_h">'content'</span><span class="sy0">;</span><br /> <br /> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="re0">$footer</span> <span class="sy0">=</span> <span class="st_h">'footer'</span><span class="sy0">;</span><br /> <span class="sy1">?></span><br /> <html><br /> <head><br /> <title>test</title><br /> </head><br /> <body><br /> <br /> <div id="header"><span class="kw2"><?php</span> <span class="kw1">echo</span> <span class="re0">$header</span><span class="sy0">;</span> <span class="sy1">?></span></div><br /> <br /> <div id="content"><span class="kw2"><?php</span> <span class="kw1">echo</span> <span class="re0">$content</span><span class="sy0">;</span> <span class="sy1">?></span></div><br /> <br /> <div id="footer"><span class="kw2"><?php</span> <span class="kw1">echo</span> <span class="re0">$footer</span><span class="sy0">;</span> <span class="sy1">?></span></div><br /> <br /> </body><br /> </html>
        </div>
      </td>
    </tr>
  </table>
</div>

注：代码里用sleep模拟服务端耗时的操作。

如果我们把串行改成并行的方式呢？每当服务器生成新的内容立刻发送给浏览器，浏览器立刻渲染，不必等到接收到全部数据再处理，毫无疑问会提升用户体验，演示代码如下：

需要说明的是代码仅运行于Apache + Mod PHP环境，旧版本Apache可能需要关闭GZip。

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
          <html><br /> <head><br /> <title>test</title><br /> </head><br /> <body><br /> <br /> <span class="kw2"><?php</span> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <div id="header"><span class="kw2"><?php</span> <span class="kw1">echo</span> <a href="http://www.php.net/str_pad"><span class="kw3">str_pad</span></a><span class="br0">&#40;</span><span class="st_h">'header'</span><span class="sy0">,</span> <span class="nu0">1024</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span></div><br /> <span class="kw2"><?php</span> <a href="http://www.php.net/ob_flush"><span class="kw3">ob_flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <br /> <span class="kw2"><?php</span> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <div id="content"><span class="kw2"><?php</span> <span class="kw1">echo</span> <a href="http://www.php.net/str_pad"><span class="kw3">str_pad</span></a><span class="br0">&#40;</span><span class="st_h">'content'</span><span class="sy0">,</span> <span class="nu0">1024</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span></div><br /> <span class="kw2"><?php</span> <a href="http://www.php.net/ob_flush"><span class="kw3">ob_flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <br /> <span class="kw2"><?php</span> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <div id="footer"><span class="kw2"><?php</span> <span class="kw1">echo</span> <a href="http://www.php.net/str_pad"><span class="kw3">str_pad</span></a><span class="br0">&#40;</span><span class="st_h">'footer'</span><span class="sy0">,</span> <span class="nu0">1024</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span></div><br /> <span class="kw2"><?php</span> <a href="http://www.php.net/ob_flush"><span class="kw3">ob_flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <br /> </body><br /> </html>
        </div>
      </td>
    </tr>
  </table>
</div>

注：某些浏览器必须接收到一定长度的内容才开始渲染，所以代码里用到了str_pad。

代码里用到ob_flush和flush把页面分块刷新缓存到浏览器，此时如果使用Firebug查看响应头的话，会发现：Transfer-Encoding=chunked，如此一来浏览器就可以实现分块渲染了。

BigPipe在此基础上更进一步，演示代码如下：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />25<br />26<br />27<br />28<br />29<br />30<br />31<br />32<br />33<br />34<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <html><br /> <head><br /> <title>test</title><br /> </head><br /> <body><br /> <br /> <div id="header"></div><br /> <br /> <div id="content"></div><br /> <br /> <div id="footer"></div><br /> <br /> <span class="kw2"><?php</span> <a href="http://www.php.net/ob_flush"><span class="kw3">ob_flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <br /> <span class="kw2"><?php</span> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="re0">$header</span> <span class="sy0">=</span> <a href="http://www.php.net/str_pad"><span class="kw3">str_pad</span></a><span class="br0">&#40;</span><span class="st_h">'header'</span><span class="sy0">,</span> <span class="nu0">1024</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <script><br /> document.getElementById("header").innerHTML = "<span class="kw2"><?php</span> <span class="kw1">echo</span> <span class="re0">$header</span><span class="sy0">;</span> <span class="sy1">?></span>";<br /> </script><br /> <span class="kw2"><?php</span> <a href="http://www.php.net/ob_flush"><span class="kw3">ob_flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <br /> <span class="kw2"><?php</span> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="re0">$content</span> <span class="sy0">=</span> <a href="http://www.php.net/str_pad"><span class="kw3">str_pad</span></a><span class="br0">&#40;</span><span class="st_h">'content'</span><span class="sy0">,</span> <span class="nu0">1024</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <script><br /> document.getElementById("content").innerHTML = "<span class="kw2"><?php</span> <span class="kw1">echo</span> <span class="re0">$content</span><span class="sy0">;</span> <span class="sy1">?></span>";<br /> </script><br /> <span class="kw2"><?php</span> <a href="http://www.php.net/ob_flush"><span class="kw3">ob_flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <br /> <span class="kw2"><?php</span> <a href="http://www.php.net/sleep"><span class="kw3">sleep</span></a><span class="br0">&#40;</span><span class="nu0">1</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="re0">$footer</span> <span class="sy0">=</span> <a href="http://www.php.net/str_pad"><span class="kw3">str_pad</span></a><span class="br0">&#40;</span><span class="st_h">'footer'</span><span class="sy0">,</span> <span class="nu0">1024</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <script><br /> document.getElementById("footer").innerHTML = "<span class="kw2"><?php</span> <span class="kw1">echo</span> <span class="re0">$footer</span><span class="sy0">;</span> <span class="sy1">?></span>";<br /> </script><br /> <span class="kw2"><?php</span> <a href="http://www.php.net/ob_flush"><span class="kw3">ob_flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <a href="http://www.php.net/flush"><span class="kw3">flush</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span> <span class="sy1">?></span><br /> <br /> </body><br /> </html>
        </div>
      </td>
    </tr>
  </table>
</div>

使用BigPipe，先刷新布局（Layout），然后按块（header，content，footer）刷新相应的Javascript代码，从而实现页面内容的填充。

BigPipe之所以使用Javascript渲染页面，是因为这样一来渲染页面的时候，就不会被块的位置束缚住，如果我们的服务器支持多线程，那么就可以同时处理多块内容，哪块先处理好就把哪块刷新到浏览器，即便不支持多线程，服务器也可以按照内容的重要程度分主次先后渲染，不必拘泥于HTML代码的物理顺序。此外还应注意一下BigPipe和Ajax二者的区别，对于一个分成若干个块的页面而言，如果使用Ajax的话，每一块都需要单独发送一个HTTP请求，而如果使用BigPipe的话，不管有多少块，都仅有一个HTTP请求。所以Ajax对服务器造成的压力会是BigPipe的若干倍。

提醒：BigPipe不利于SEO，应用时可通过User Agent判断请求是人还是搜索引擎，如果是人的话，则应用BigPipe渲染模式，如果是搜索引擎的话，则应用传统渲染模式。

补充：在Nginx + PHP FastCGI环境运行文中的代码，会发现无效，这是缓存造成的。在Nginx FastCGI环境下，如果数据小于fastcgi\_buffers，会缓存到内存中，否则如果数据小于fastcgi\_max\_temp\_file\_size，会缓存到硬盘上。因为flush是Apache环境下才有效的函数，不适用于Nginx环境，所以唯一的出路就是想办法关闭缓存，可通过实验发现即便把fastcgi\_buffers和fastcgi\_max\_temp\_file\_size都禁止了，还是没有用，所以说截至目前为止，Nginx + PHP FastCG无法实现BigPipe，相对可行的方法是通过Apache + Mod PHP实现BigPipe，而Nginx则放在代理服务器的角色上，并使用proxy_buffering关闭代理缓存。

参考：Facebook网站的Ajax化、缓存和流水线（PDF）。  
来源：http://huoding.com/2011/06/26/88
