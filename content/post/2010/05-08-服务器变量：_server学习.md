---
date: Sat, 08 May 2010 15:41:23 +0000
title: 服务器变量：$_SERVER学习
author: tomheng
layout: post
permalink: /archives/653.html
robotsmeta:
  - index,follow
views: 556
categories:
  - LAMP
tags:
  - PHP基础
---
经常使用这个全局变量，但是对$_SERVER里一些细节不是很清晰，特在此做个总结。

**服务器变量：**<tt><strong>$_SERVER介绍</strong></tt>

> <tt>$_SERVER</tt> 是一个包含诸如头部(<span style="color: #ff0000;">headers</span>)、路径(<span style="color: #ff0000;">paths</span>)和脚本位置(<span style="color: #ff0000;">script locations</span>)的数组。数组的实体由 web 服务器创建。不能保证所有的服务器都能产生所有的信息；服务器可能忽略了一些信息，或者产生了一些未在下面列出的新的信息。这意味着，大量的这些变量在 <a href="http://hoohoo.ncsa.uiuc.edu/cgi/env.html" target="_top">CGI 1.1 specification</a> 中说明，所以您应该仔细研究它。

**直接用实例来说明，比较直接一些**

<span style="color: #999999;"> URL</span>:  <span style="color: #ff0000;">http://www.yholiday.com/index.php/hello/tomheng/?hobby=php</span>

  1. <span style="color: #ff0000;"><span style="color: #0000ff;">$_SERVER[&#8216;</span><tt><span style="color: #000000;"><span style="color: #0000ff;">REQUEST_URI']</span>='http://www.yholiday.com/index.php/hello/tomheng/'</span></tt></span>
  2. <span style="color: #0000ff;">$_SERVER[&#8216;</span><tt><span style="color: #0000ff;">REQUEST_URI']</span>='/index.php/hello/tomheng/'</tt>
  3. <span style="color: #0000ff;">$_SERVER[&#8216;SCRIPT_NAME</span><tt><span style="color: #0000ff;">']</span>='index.php'</tt>
  4. <span style="color: #0000ff;">$_SERVER[&#8216;DOCUMENT_ROOT</span><tt><span style="color: #0000ff;">']</span>=http.conf中设置的DOCUMENT_ROOT位置</tt>
  5. <span style="color: #0000ff;">$_SERVER[&#8216;SCRIPT_FILENAME</span><tt><span style="color: #0000ff;">']</span>=$_SERVER['DOCUMENT_ROOT<tt>'].$_SERVER['SCRIPT_NAME<tt>']</tt></tt></tt>
  6. <span style="color: #0000ff;">$_SERVER[&#8216;PATH_INFO</span><tt><span style="color: #0000ff;">']</span>='/hello/tomheng/'</tt>
  7. <span style="color: #0000ff;">$_SERVER[&#8216;QUERY_STRING</span><tt><span style="color: #0000ff;">']</span>='hobby=php'</tt>

<span style="color: #ff0000;"><strong>说明</strong></span>：$\_SERVER[&#8216;PATH\_INFO<tt>']变量是指<span style="color: #ff0000;">$_SERVER['SCRIPT_NAME</span><tt><span style="color: #ff0000;">']</span>以后"<span style="color: #ff0000;">?</span>"以前的内容，这个值常用于模拟rewrite效果，实现的URL也很漂亮。</tt></tt>
