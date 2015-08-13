---
date: Thu, 17 Sep 2009 16:25:11 +0000
title: Session中的SID 变量
author: tomheng
layout: post
permalink: /archives/156.html
views: 554
categories:
  - LAMP
---
Php 默认将session 存储在文件中，启动session 时会得到一个标识符。前段时间改程序中对这个标识符和session 有了新的认识，所以借此总结一下。  
SID 是个静态变量可以得到生成的phpssessionid 这个id 是session\_start()产生的，或者认为是生成session文件的标志。在初次使用session\_start()时SID是有值的，$GLOBALS\[&#8216;_COOKIE&#8217;\]\[&#8216;PHPSESSID&#8217;\]无值得，刷新之后颠倒。逻辑应该是这样的，session开启的时候，先检查cookie中是否有PHPSESSID，如果有在依据此id去寻找session文件，解析数据形成session 变量；如果cookie中没有这个id，那么就生成一个id串并把这个id注册到SID中，然后创建文件。没有看php源码，偶也看不懂，如有不对欢迎指正。

<pre lang="php">session_start();
echo SID;

var_dump($GLOBALS['_COOKIE']['PHPSESSID']);</pre>

;  
刷新页面看看看
