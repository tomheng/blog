---
date: Wed, 03 Aug 2011 16:01:11 +0000
title: PHP-FPM一些特点
author: tomheng
layout: post
permalink: /archives/1156.html
views: 670
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - FPM
  - PHP
---
<big><strong>PHP-FPM: PHP FastCGI 进程管理器</strong></big>

PHP-FPM 是一个用以极大地改进 FastCGI SAPI 在生产环境中使用的 PHP4/5 补丁,PHP5.3.3已经包含PHP-FPM的支持。

今天读到的一个网页，介绍了一些FPM的一些特性。

文中介绍的Error\_Header 、优化上传支持、fastcgi\_finish\_request、request\_slowlog_timeout都很用。

其中后面的两个可以用来提高网站的响应速度，个人比较感兴趣，有时间要做个试验看看情况。

详细介绍：<http://php-fpm.org/wiki/CN:Features>
