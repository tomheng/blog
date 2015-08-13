---
date: Tue, 09 Jun 2009 07:15:21 +0000
title: php检测服务器运行状况（ping和http）
author: tomheng
layout: post
permalink: /archives/39.html
views: 1175
categories:
  - LAMP
---
## 一、概况：

*名称*：websiteMonitor1.0

*作者*：tomheng

*时间*：2009-6-2

*功能*：通过http 和ping的方式检测服务器的运行情况，并且记录文档。

下载：[地址][1]

## 二、实现分析：

1）主要用到了php的两个函数：exec()和fscockopen()。

string exec ( string command [, array output [, int return_var]])。函数的具体使用说明参阅php手册，这个函数可以向服务器发送一个可执行的命令，在此处我们向服务器发送了一个ping指令，exec()将执行的结果返回过来，这里有一点很重要就是他有两个地方可以发返回结果，

$result=exec($cd,$output,$status);$result是返回的最后一行结果，$output返回的是所有的结果：比如：ping测试的结果如下：

<div id=&#8221;part1&#8243;>

Reply from 203.208.37.104: bytes=32 time=35ms TTL=239

Reply from 203.208.37.104: bytes=32 time=239ms TTL=239

Reply from 203.208.37.104: bytes=32 time=234ms TTL=239

Reply from 203.208.37.104: bytes=32 time=209ms TTL=239

 

Ping statistics for 203.208.37.104:

    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),

Approximate round trip times in milli-seconds:

</div>

<div id=&#8221;part2&#8243;>

Minimum = 35ms, Maximum = 239ms, Average = 179ms

</div>

$result=#part1

$output=#part2

Fsockopen()没有什么特别的，此处就是打开一个http连接并且读取返回的状态代码：

2)这应该算是一个cron或者守护进程，计划任务之类的东西，因此这个网页一旦打开，只要服务器（指放置此代码的服务器）不down掉，他就一直运行下去。所以一旦打开运行此程序，我们无法关闭此程序，记录日志的文件将一直增加，但你可以通过删除日志文件的方法来减少磁盘的占用。

## 三、基本用法：

把此代码放在一个支持php的服务器上，然后填写另一个主机(您想检测的）的ip或者域名就可以实现对另一台主机的检测。

 [1]: http://sharecode2php.googlecode.com/files/monitor.rar
