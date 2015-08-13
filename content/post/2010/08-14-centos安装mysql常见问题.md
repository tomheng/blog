---
date: Sat, 14 Aug 2010 09:27:25 +0000
title: CentOS安装MySQL常见问题
author: tomheng
layout: post
permalink: /archives/771.html
robotsmeta:
  - index,follow
views: 605
categories:
  - LAMP
tags:
  - Linux
---
在CentOS继续折腾MySQL的编译安装······，安装过程中常遇到gcc编译器缺失和No curses/termcap library found问题。解决方案总结如下

**1）gcc编译器缺失**

运行命令：<span style="color: #3366ff;">yum install gcc-c++</span>

**2）No curses/termcap library found**

通过命令安装** **curses/termcap库 <span style="color: #3366ff;">yum install ncurses-devel</span>
