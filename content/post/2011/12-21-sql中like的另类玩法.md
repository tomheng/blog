---
date: Wed, 21 Dec 2011 11:48:27 +0000
title: SQL中LIKE的另类玩法
author: tomheng
layout: post
permalink: /archives/1254.html
robotsmeta:
  - index,follow
views: 335
categories:
  - LAMP
tags:
  - MySQL
---
设想一个场景，我有个关键词屏蔽词库（应该很多网站都有吧），所有的关键词都存储在MySQL中，怎样查询某个字符串是否含有屏蔽关键词？

比如我们有一个屏蔽词 tom，要屏蔽任何含有tom的词语。

通常情况下LIKE这样用

<span style="color: #0000ff;">SELECT * FROM `table` WHERE field LIKE &#8216;%字符串%&#8217;</span>

下面我们这样玩，来解决上面的问题

<span style="color: #0000ff;">SELECT * FROM `table` WHERE &#8216;字符串&#8217; LIKE CONCAT(&#8216;%&#8217;,field,&#8217;%&#8217;)</span>

PS:

这让我想起了初中时数学课中经常用来做几何证明的逆向思考方式。
