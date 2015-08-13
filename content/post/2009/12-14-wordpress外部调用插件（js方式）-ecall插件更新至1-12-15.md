---
date: Mon, 14 Dec 2009 09:43:00 +0000
title: WordPress外部调用插件（js方式）-Ecall插件更新至1.12.15
author: tomheng
layout: post
permalink: /archives/358.html
keywords: Wordpress外部调用插件（js方式）-Ecall插件
description: Wordpress外部调用插件（js方式）-Ecall插件
robotsmeta:
  - index,follow
views: 1501
categories:
  - My project
tags:
  - JavaScript
  - 插件
---
<a class="wpgallery" title="wordpress插件Ecall-js方式外部调用wordpress文章" href="http://blog.webfuns.net/archives/313.html" target="_self">wordpress外部调用插件-Ecall插件主页</a>

**（1）主要更新：改变外部调用的方式**

原先是在根目录下建立一个文件，外部调用这个文件来实现文章的外部调用。这种方式有很多的弊端，首先是有的虚拟机主机可能目录权限不够，导致程序不能把api.php文件拷贝至根目录，只能手动来拷贝文件。另外还可能产生安全性和效率上的问题。

现在采用的hook方式，截获url，分析如果是外部调用的url则进行处理，否则放行，由WordPress进行正常的处理。

现在的调用方式改为：

<script type=&#8217;text/javascript&#8217; src=&#8217;http://domain/index.php?key=15e3f539603bee92e0d5c6f2718a02e3&cid=0&rows=6&len=4&#8242;></script>

或者

<script type=&#8217;text/javascript&#8217; src=&#8217;http://domain/?key=15e3f539603bee92e0d5c6f2718a02e3&cid=0&rows=6&len=4&#8242;></script>

**（2）调用代码支持使用len参数来控制标题的字符数目**

Len是个可选的参数可以不添加，默认值为120个字节。截取字符支持单字节和双字节混合截取。

可以再调用的代码后面加len=?来控制标题字符的显示数目

<script type=&#8217;text/javascript&#8217; src=&#8217;http://domain/index.php?key=15e3f539603bee92e0d5c6f2718a02e3&cid=0&rows=6&len=4&#8242;></script>
