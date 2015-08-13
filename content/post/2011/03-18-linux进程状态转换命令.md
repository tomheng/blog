---
date: Fri, 18 Mar 2011 11:25:10 +0000
title: Linux进程相关命令整理
author: tomheng
layout: post
permalink: /archives/993.html
robotsmeta:
  - index,follow
views: 592
categories:
  - LAMP
tags:
  - shell
---
<span style="color: #ff0000;">君欲善其事，必先利其器！</span>

1）<span style="color: #008000;">&</span>  
这个用在一个命令的最后，可以把这个命令放到后台执行

2）<span style="color: #008000;">ctrl + z</span>  
可以将一个正在前台执行的命令放到后台，并且暂停

3）<span style="color: #008000;">jobs</span>  
查看当前有多少在后台运行的命令

4）<span style="color: #008000;">fg</span>  
将后台中的命令调至前台继续运行  
如果后台中有多个命令，可以用 fg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)

5）<span style="color: #008000;">bg</span>  
将一个在后台暂停的命令，变成继续执行  
如果后台中有多个命令，可以用bg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)

6)<span style="color: #008000;">ctrl+c</span>  
终止前台进程

7)<span style="color: #008000;">ps</span>  
查看进程信息

8）<span style="color: #008000;">kill</span>  
kill 管理后台的任务.

9)<span style="color: #008000;">nohup</span>  
用它在后台运行一个命令，即使在用户退出时也不受影响
