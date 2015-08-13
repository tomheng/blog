---
date: Fri, 06 Jun 2014 05:47:07 +0000
title: ubuntu 过热关机保护的问题
author: tomheng
layout: post
permalink: /archives/1796.html
views: 150
categories:
  - LAMP
  - My project
tags:
  - Ubuntu
---
不知道Ubuntu 从哪个版本开始加上的CPU的过热保护机制，就是CPU一旦到达指定温度（我的是90度），啪就自己关机。

这对于我那个用了快5年的v460简直悲剧，运行进程多了、或者像现在天气热了，分分钟自己关掉。在网上找了半天，终于找到如何关掉这个闹人的过热保护机制了。

打开/etc/default/grub.cfg文件，找到 GRUB\_CMDLINE\_LINUX\_DEFAULT,改成GRUB\_CMDLINE\_LINUX\_DEFAULT=&#8221;quiet splash thermal.nocrt=1&#8243;。然后sudo update-grub,重启让自己的本本疯狂燃烧吧~

<span style="color: #ff6600;">注：毕竟这个设置是对CPU的一种保护，去掉之后有可能把本本烧残。</span>
