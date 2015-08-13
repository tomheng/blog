---
date: Mon, 14 May 2012 05:05:17 +0000
title: ubuntu更换网卡后无法联网,提示no such divice
author: tomheng
layout: post
permalink: /archives/1530.html
views: 326
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - Ubuntu
---
今天在virtualbox中新建了一个系统，虚拟硬盘使用的是已有的虚拟硬盘，创建完启动后，无法联网！这是因为/etc/udev/rules.d/70-persistent-net.rules文件中保存了网卡的相关信息没有更新，而是把新系统的网卡作为一个新设备(比如：eth2)放到此文件中，但是你系统中仍然使用的是以前的那块网卡（比如eth1)。

所以解决的方法有两个：

1）删除/etc/udev/rules.d/70-persistent-net.rules文件，然后重启就可以了

<span style="color: #0000ff;">sudo rm  /etc/udev/rules.d/70-persistent-net.rules  && sudo reboot</span>

2）修改/etc/network/interface中的网卡为新加入的网卡

&nbsp;
