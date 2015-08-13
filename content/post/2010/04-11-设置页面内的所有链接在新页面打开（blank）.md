---
date: Sun, 11 Apr 2010 14:17:36 +0000
title: 设置页面内的所有链接在新页面打开（blank）
author: tomheng
layout: post
permalink: /archives/567.html
views: 2319
keywords: 新页面打开连接,blank
description: 今天遇到一个需求，原先页面设计的时候，设计的一个页面是在同一个网页中打开，可是后来发生变化，要求页面中所有链接在新页面中打开。
robotsmeta:
  - index,follow
categories:
  - WordPress
tags:
  - SEO
---
今天遇到一个需求，原先页面设计的时候，设计的一个页面是在同一个网页中打开，可是后来发生变化，要求页面中所有链接在新页面中打开。可是如果逐个修改页面中链接效率是极其底的，于是想到能否通过向css样式一样的方式来实现页内的所有链接全部在新页面内打开，在搜索引擎中搜索一下，果然有这样的解决方案。

最终发现了一个这样神奇的标签，只需在网页head加入如下代码。网页中所有的链接就可以在新页面中打开了。

<span style="color: #ff00ff;"><base target=&#8221;_blank&#8221;/></span>

<span style="color: #000000;">经测试，这个方法在Firefox，Chrome，IE（ie6）内核的浏览器中都有效。</span>

<span style="color: #ff00ff;"><strong><span style="color: #000000;">总结：</span></strong></span>

<span style="color: #000000;">就是如此简单的<a href="http://www.w3school.com.cn/tags/tag_base.asp">base标签</a>，就可以实现我所要实现的效果，着实减轻了我的负担。HTML中还是有很多标签功能是很强大的，过段时间有必要自习复习一下HTML中的标签。看来每一项看似简单的技术，真的要做的极致，确实是需要长时间学习的。</span>
