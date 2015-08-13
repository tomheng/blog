---
date: Sun, 26 Sep 2010 11:11:52 +0000
title: 再谈MySQL乱码问题
author: tomheng
layout: post
permalink: /archives/828.html
keywords: MySQL乱码
robotsmeta:
  - index,follow
views: 753
categories:
  - LAMP
tags:
  - MySQL
---
**MySQL乱码问题**确实是一个比较头痛的问题。

前些天又遇到了[MySQL][1]的乱码问题，本来我自己做网站的话，现在不会遇到乱码。但是前些天接手的一个网站，前期是一个老师弄了一段时间，然后又找我帮忙弄得，所以到最后迁移的时候又遇到了的编码问题。

在解决的时候找到了一个办法。场景是这样的，我们是用[DEDE][2]做的网站，原先使用GBK做的，后来要迁移到UTF8上来。我们知道直接在数据库中改变编码会导致数据库中的数据直接在数据库中存储的都是乱码，所以我这样解决的问题，用[phpMyAdmin][3]的导出功能导出GBK编码的数据，然后打开导出的SQL文件，然后搜索替换里面的GBK关键词为UTF8，然后接SQL导出文件导入到目标机器。这样就可以平滑的将数据库由GBK变为UTF8的编码。

 [1]: http://www.mysql.com/
 [2]: http://www.dedecms.com/
 [3]: http://www.phpmyadmin.net/
