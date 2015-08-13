---
date: Sun, 22 Nov 2009 12:31:50 +0000
title: php读取mysql中文乱码问题
author: tomheng
layout: post
permalink: /archives/303.html
keywords: 'php   mysql   中文乱码问题'
description: php读取mysql中文乱码问题的原因分析和解决方法
views: 1202
categories:
  - LAMP
tags:
  - LAMP
  - MySQL
---
<a class="wpgallery" title="php中文乱码问题" href="http://blog.webfuns.net/archives/288.html" target="_blank">php中文乱码问题</a>几乎是每一个php初学者都要遇到的问题，php中文乱码问题最典型的就是php读取<a class="wpgallery" title="mysql" href="http://mysql.com" target="_blank">mysql</a>或向mysql写入数据的时候出现的中文乱码问题。出现php中文乱码问题的时候一般可以通过一下两种方法解决：

(1) **最简单的修改方法，就是修改mysql的my.ini文件中的字符集键值。**

如 default-character-set = utf8

character\_set\_server = utf8

修改完后，重启mysql的服务，service mysql restart

（2）**发送查询query 查询前发送SET NAMES &#8216;utf8&#8217;语句。**

mysql_query(&#8220;set names utf8&#8243;);

之后就可以正常的发送自己的查询语句了。

通过上面的两个方法一般就可以解决<a class="wpgallery" title="php" href="http://php.net" target="_blank">php</a>读取mysql中文乱码的问题
