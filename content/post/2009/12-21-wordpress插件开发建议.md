---
date: Mon, 21 Dec 2009 00:26:14 +0000
title: wordpress插件开发建议
author: tomheng
layout: post
permalink: /archives/392.html
keywords: wordpress 插件开发 建议
views: 631
categories:
  - WordPress
tags:
  - 插件
  - 规范
---
<span style="color: #ff6600;">本文属于翻译文章</span>  
<span style="color: #ff6600;">原文</span>：<a class="wp-oembed" title="Plugin Development Suggestions" href="http://codex.wordpress.org/Writing_a_Plugin" target="_blank">Plugin Development Suggestions</a> 

（1）<a class="wpGallery" title="wordpress" href="http://wordpress.org" target="_blank">wordpress</a>插件的代码都应该遵循wordpress的代码规范。

请同时考虑内置文档规范。

（2）插件中所有函数的命名都需要区别wordpress核心代码，其他插件和主题中函数的命名。鉴于此，对插件中的所有函数使用一个唯一的函数名前缀是一种好的做法；此外，还可以把函数定义在类内（同样需要一个唯一的名字）

（3）在插件中不要对数据库表的前缀进行硬编码（例如&#8221;wp_&#8221;)，一定要使用$wpdb->prefix变量来替代。

（4）数据库的读操作是廉价的，但写操作是昂贵的。数据库最擅长的就是取出数据，然后返回给你，这些操作都是很轻快的。然而对数据库的改变却是一个复杂的过程，并且需要昂贵的计算。所以，要尽量减少对数据库的写入操作。在代码中尽量做好准备，当你需要的时候你可以专注与这些写操作。

（5）只获得你所需要的数据。尽管数据库返回数据非常迅速，但是你仍然需要通过只获取自己需要的内容来尽量减轻数据库的负担。如果需要统计数据表中数据的行数，不要使用“SELECT * FORM ”这种语句，因为每一行的每一个数据都要被取出来，很消耗内存。同样，如果你在插件中仅需要post\_id和post\_author,那么就只取出这些字段就可以啦，尽量减轻数据库的负担。记住：在同一时刻有成千上万的其他的进程在访问数据库。数据库和服务器要把他们仅有的资源分散到所有的进程中。学会如何减少对数据库的访问将确保你的插件不会被指责成滥用资源的插件。

（6）消除插件中的所有错误。在wp-config.php中添加 define(&#8216;WP_DEBUG&#8217;, true)。测试插件中的每一个功能来检查插件中是否存在错误或者警告。修复这些错误，继续调试直到所有的错误被消除。
