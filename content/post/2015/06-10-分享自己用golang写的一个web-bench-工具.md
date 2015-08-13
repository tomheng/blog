---
date: Wed, 10 Jun 2015 12:49:08 +0000
title: 分享自己用Golang写的一个Web bench 工具
author: tomheng
layout: post
permalink: /archives/1934.html
views: 117
categories:
  - LAMP
tags:
  - Golang
---
最近一直在学习Golang，业余时间开了个Web bench 工具awb，awb是another web bench的缩写，基本功能已完成，兼容ab的参数。

初步测试并发性比ab要好一些。感觉Golang除了在服务端开发方面有很大的优势以外，在写各类工具方面也是挺方便顺手的。估计未来会有很多写的运维工具使用Golang 编写。

总体上来说Go非常符合我的胃口。既有PHP的简洁、高效，在性能、并发、异步方面又有很好的系统级别的支持。整体上很有当年第一次看到PHP的时候的感觉。。。心动不已啊 <img src="http://blog.webfuns.net/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

项目地址：https://github.com/tomheng/awb
