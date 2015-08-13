---
date: Fri, 11 Oct 2013 14:07:51 +0000
title: 轻松打造自己的Git Server
author: tomheng
layout: post
permalink: /archives/1746.html
views: 1266
categories:
  - LAMP
tags:
  - git
---
[Git][1] 作为分布式的版本管理系统，虽然没有中心Git Server也能很好的工作，但是搭建一个属于自己的Git Server会让自己的代码更安全。其实搭建的步骤非常简单，只需要如下几步就可以享受自己私有的Git Server啦！

当然一切的前提是你要有一个自己Server， 推荐大家试试[DigitalOcean SSD Cloud VPS Server][2]，很靠谱。

**一、安装Git**

用<span style="color: #003366;">sudo apt-get install git-core</span>命令在自己的Server和本地计算机上安装Git，当然如果喜欢也可以自己通过源代码编译安装。

**二、建立Git账户**

首先在Server端通过<span style="color: #003366;">sudo adduser git</span> 命令添加git账户，然后根据[这篇文章][3]介绍添加ssh公钥。操作完成后，应该可以用命令<span style="color: #003366;">ssh git@server.com</span>直接登录到自己的Server。

**三、初始化Server仓库**

  1. ssh git@server.com #登录到自己Server
  2. <span style="line-height: 1.714285714; font-size: 1rem;">mkdir myrepo.git #创建测试库目录</span>
  3. cd myrepo.git ; git &#8211;bare init

**四、本地检出代码**

  1. mkdir myrepo #建立空目录
  2. git clone git@server.com:myrepo.git #检出代码

over现在一个自己私有的Git Server就建好了，可以自有Git It啦～。

附录：[git &#8211; 简易指南][4]

 [1]: http://blog.webfuns.net/archives/1379.html "初识Git"
 [2]: http://blog.webfuns.net/archives/1687.html "DigitalOcean SSD Cloud  VPS Server 你值得拥有"
 [3]: http://blog.webfuns.net/archives/1468.html "SSH实现免密码登录"
 [4]: http://rogerdudler.github.io/git-guide/index.zh.html "git - 简易指南"
