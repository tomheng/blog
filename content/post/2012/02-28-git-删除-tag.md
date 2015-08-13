---
date: Tue, 28 Feb 2012 02:52:52 +0000
title: git 删除 tag
author: tomheng
layout: post
permalink: /archives/1419.html
views: 711
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - git
---
删除本地：

git  tag -d v1.1

删除远程：

git push origin :refs/tags/v1.1

注意：

“:refs” 前面要有空格

&nbsp;
