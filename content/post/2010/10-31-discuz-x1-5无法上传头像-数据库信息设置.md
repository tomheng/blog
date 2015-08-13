---
date: Sun, 31 Oct 2010 13:07:33 +0000
title: discuz x1.5无法上传头像-数据库信息设置
author: tomheng
layout: post
permalink: /archives/879.html
keywords: discuz x1.5无法上传头像
description: 嗯，discuz x1.5确实强大，前段时间安装了一个。后台看还具有门户的作用，也就是可以用X1.5做出一个大型的门户网站。对于这一点我不太看好，论坛就要像论坛，门户就要像门户，这山看着那山高，什么也做不好。
robotsmeta:
  - index,follow
views: 1364
categories:
  - LAMP
tags:
  - discuz
  - PHP
---
<span style="color: #ff0000;">这是本博客第111篇文章，本想流到光棍节发，想想还是别留了，有了东西还是要早写出来。</span>

嗯，[discuz x1.5][1]确实强大，前段时间安装了一个。后台看还具有门户的作用，也就是可以用X1.5做出一个大型的门户网站。对于这一点我不太看好，论坛就要像论坛，门户就要像门户，这山看着那山高，什么也做不好。就像WordPress开始向CMS转变一样，弄得自己么有了特点。本事[WordPress][2]作用一个典型的博客程序，在用户心中已经建立了品牌，在塞入一个CMS，最终会让大家搞不清WordPress到底是干什么的。discuz也是做论坛出什么就是做论坛，为什么要搞出个门户功能混进去，要做也可以，可以做一个独立的项目区试试。以前提到discuz都知道是论坛程序，以后discuz是什么，论坛or门户，结论杂种。就是这样，看看以后他们的发展吧···

有些跑题，说说discuz x1.5无法上传头像问题，我以前做过一次论坛的迁移，迁移成功，论坛可以正常访问，但是我今天试图去上传头像时，发现无法上传头像。提示ucenter can not connect to MYsql server,不过我已经迁移成功怎么可能连不上数据库呢？

于是到/bbs/config 中查看config\_global.php 和config\_ucenter.php的数据库信息，都是现在正在使用的数据库信息。纳闷，到底是哪里的原因呢，难道是存在数据库中，于是到数据库中招了一番，也没有发现。最好发现在/bbs/uc_server/data/config.inc.php 还有数据库配置，就是这里啦，看来真正的ucenter的配置信息时存在这里的。

不过很纳闷/bbs/config/config_ucenter.php这个文件中的ucenter数据库信息时干什么的？

 [1]: http://www.discuz.net/release/dzx15/?ac=index
 [2]: http://www.wordpress.org
