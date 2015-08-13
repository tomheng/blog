---
date: Mon, 21 Nov 2011 12:03:53 +0000
title: rename 函数和 copy 函数注意事项
author: tomheng
layout: post
permalink: /archives/1203.html
views: 645
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - PHP
---
rename 函数和 copy函数都是尝试进行操作。

rename是尝试重命名文件，copy是尝试复制文件，两者都是成功返回TRUE，失败返回FALSE。

因为是尝试操作，所有当目录下已经存在同名的文件时并不会去执行操作，从而返回FALSE。

如果想要尽量执行操作，而忽略同名文件，那么之前可以unlink一下。

&#8211;END&#8211;
