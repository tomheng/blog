---
date: Thu, 28 Oct 2010 02:13:32 +0000
title: 错误101 (net::ERR_CONNECTION_RESET):是什么意思？
author: tomheng
layout: post
permalink: /archives/872.html
robotsmeta:
  - index,follow
views: 27301
categories:
  - LAMP
tags:
  - PHP基础
---
如果访问网站的时候，遇到这个错误101 (net::ERR\_CONNECTION\_RESET):，那么恭喜撞墙（防火墙 Firewall）了。今天在使用PHP的本地运行环境时，遇到这个问题。后来找到原因是防火墙的问题，只要把防火墙关闭就可以解决。

顺便说一下如何在Win7中关闭防火墙：

在开始菜单中打开运行(win+R快捷键)：services.msc，然后就可以看到在后台运行的服务，找到Windows Firewall 选择属性设置为禁止即可。

<span style="color: #ff0000;">PS:关了防火墙解决了部分问题，但是WordPress还是在点击后台的部分链接时出错。后来知道是我的PHP运行环境没有安装好，原先用的wamp，后来换成了xampp就Ok了。</span>
