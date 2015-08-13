---
date: Sun, 19 Jun 2011 03:30:36 +0000
title: Linux下使用find命令批量删除文件
author: tomheng
layout: post
permalink: /archives/1067.html
views: 1460
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - Linux
---
想找一个批量删除一类文件得命令，刚开始想到的是rm，但是查了一下发现rm命令不能模糊查找，最后find命令可以满足需求。

命令如下：

<span style="color: #0000ff;">find  . -name &#8216;*.swp&#8217; -exec rm {} \;</span>

<span style="color: #ff0000;">PS：这种命令很危险，用的时候要小心</span>
