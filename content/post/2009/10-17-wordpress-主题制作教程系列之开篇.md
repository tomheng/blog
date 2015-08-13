---
date: Sat, 17 Oct 2009 11:00:19 +0000
title: wordpress 主题制作教程系列(1)之开篇
author: tomheng
layout: post
permalink: /archives/169.html
views: 811
categories:
  - WordPress
tags:
  - WordPress
---
### wordpress 主题制作教程简单说明

为了能使WordPress fans制作富有个性的wordpress主题，经过精心的准备，<a class="cflabel" title="WordPress教程网" href="http://www.wpcourse.com" target="_blank">wpc</a>终于推出了这套WordPress主题制作入门教程！

<a class="cflabel" title="wordpress" href="http://wordpress.org" target="_blank">WordPress</a>是一个很优秀的开源博客程序，受到全世界网民的追捧。它不但是一个简洁易用的博客应用程序，同时也是个学习的好工具。通过主题的制作你可以学习网页设计相关知识，学习插件开发则可以提高你php编程的能力，如果更深入的了解一下WordPress的框架结构，那对你架构设计能力也是一种提高。我们提供的这套入门教程旨在普及WordPress主题制作的基础知识，面向最基础的入门用户，让大家认识到制作WordPress主题是简单的，有趣的（<span style="color: #0000ff">It is easy ,it is fun这是我们的口号</span>)。我们的教程会讲解如何制作WordPress 主题，但是我们的教程不会告诉您如何制作漂亮的主题。

学习我们的教程需要您首先的是一个WordPress的熟练的用户，如果你还不是很熟悉如何使用WordPress，请移步WordPress教程网查看<a class="cflabel" title="Wordpress视频教程 " href="http://www.wpcourse.com/menu" target="_blank">WordPress的基本的使用教程</a>。

附注：本教程是WordPress主题制作教程@wpc的附属版本，这是我根据老五的要求写的第一个版本，但是被老五打回来啦，但是我又不想浪费此版本，特在我的博客上发布，也许会对大家有所帮助。大家可以和wpc版本对照着看，喜欢WordPress的朋友WordPress教程网确实是个学习WordPress的好地方。

<span style="color: #ff6600">教程开始之前还是要说一些网页设计的基本的规范以及一些基本的php常识：</span>

<span style="font-weight: normal"><strong>（1）</strong></span>**所有html 标签必须关闭**

<ul></ul>

（2）**Html标签的不能交叉嵌套**

<div><span></div></span> <span style="color: #ff0000">这样是不允许的</span>

<div><span></span></div> <span style="color: #ff0000">这样是正确的</span>

（3）**凡是在<?php ···?>或者在<? ···?>里的内容都是php代码**

（4）**Php 的两种注释方法**

// 单行注释

/**/ 跨行注释

即以/\*开头到\*/终止的中间部分全部为注释代码

5）**css 样式文件的注释方法 只有一种既 /**/ 跨行注释**

<span style="color: #ff6600">推荐一些工具和网站</span>

1）php不懂得问题去。<a class="cflabel" title="php" href="http://php.net/" target="_blank">Php.net</a>查询

2）与css，html有关的问题去：<a class="cflabel" title="w3school" href="http://www.w3school.com.cn/" target="_blank">w3school</a>查询
