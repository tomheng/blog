---
date: Sun, 06 Dec 2009 17:35:59 +0000
title: js方式外部调用wordpress文章插件Ecall更新1.12.7版本
author: tomheng
layout: post
permalink: /archives/341.html
keywords: 'wordpree 插件  javascript站外调用'
views: 2143
categories:
  - My project
tags:
  - JavaScript
  - WordPress
  - 插件
---
js方式外部调用<a class="wpgallery" title="wordpress" href="http://wordpress.org" target="_blank">wordpress</a>文章插件Ecall已更新到1.12.7版本

下载 <a class="wpgallery" title="js外部调用wordpress文章" href="http://wordpress.org/extend/plugins/ecall/" target="_blank">wordpress Ecall</a> <a title="wordpress插件Ecall-js方式外部调用wordpress文章" href="http://blog.webfuns.net/archives/313.html" target="_blank">插件主页</a>

<span style="color: #ff6600;"><strong>更新日志</strong></span>

（1）**调整了cid为0时的bug。修改后cid=0和隐藏分类组合可以使wordpress站外调用更灵活**

目前cid有以下几种用法。

A)<span style="color: #008080;">cid=某个分类的id</span>

示例：

<span style="color: #0000ff;"><script type=&#8217;text/javascript&#8217; src=&#8217;http://blog.webfuns.net/api.php?key=b32154b43d6332afcb130b3a633c6ce4&cid=7&rows=6&#8242;></script></span>

这样可以实现站外调用某个具体分类下的文章

B)<span style="color: #008080;">cid=0 不隐藏任何分类</span>

<span style="color: #0000ff;"><script type=&#8217;text/javascript&#8217; src=&#8217;http://blog.webfuns.net/api.php?key=b32154b43d6332afcb130b3a633c6ce4&cid=0&rows=6&#8242;></script></span>

这样可以站外调用所有的分类下的文章

C)<span style="color: #008080;">cid=0 隐藏某些分类</span>

以趣味互联网为例：

Google(cid=1)

WordPress(cid=2）

LAMP（cid=3)

My Project(cid=4)

如果把Google隐藏了，

<span style="color: #0000ff;"><script type=&#8217;text/javascript&#8217; src=&#8217;http://blog.webfuns.net/api.php?key=b32154b43d6332afcb130b3a633c6ce4&cid=0&rows=6&#8242;></script></span>

这样站外调用将显示Wordpress、LAMP、My Project三个分类下的文章

如果再把Wordpress隐藏了，就显示LAMP、My Project两个分类下的文章，以此类推，如果全部隐藏了，就不会显示任何调用。

2)  **增加了错误提示**

3)  **修复了Cache time 无法设置为空的问题**

4）**修复了隐藏目录不能全部同时取消的问题**

5）**模板中不再支持content调用**

原因是大文本调用容易出现不可预测的字符，从而导致js调用出错，此外大文本的外部调用浪费系统资源且价值不是很大。
