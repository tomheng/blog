---
date: Tue, 01 Dec 2009 17:06:25 +0000
title: linux桌面系统ubuntu 9.10常见问题-使用体验
author: tomheng
layout: post
permalink: /archives/330.html
keywords: linux ubuntu 9.10 常见问题
description: linux桌面系统ubuntu 9.10常见问题-使用体验
views: 848
categories:
  - LAMP
tags:
  - Linux
  - Ubuntu9.10
---
<div style="width: 510px" class="wp-caption aligncenter">
  <img title="ubuntu" src="http://www.ruanyifeng.com/blog/2007/07/ubuntu.jpg" alt="ubuntu" width="500" height="375" />
  
  <p class="wp-caption-text">
    ubuntu
  </p>
</div>

ubuntu9.10发布有一段时间了，出于个人爱好和以后发展需要，上周末痛下决心把windows格掉，装上

（1）<span style="color: #000000;"><strong>别扭</strong></span>  
一直用windows养成的习惯，上来就找我的电脑，汗。Linux 桌面布局和windows不一样，找很多东西都不习惯，像迷途的羔羊，找不到东西南北,一直出于迷路状态。这也是我使用ubuntu的原因之一，就是要改变在windows下养成的习惯。  
（2）**中文问题**  
官方下载的版本对中文的支持并不是很好，默认的中文输入法很糟糕，我的e文又不好，所以首要的就是要换一种比较顺手高效的中文输入法，<a class="wpgallery" title="IBUS" href="http://www.google.cn/search?hl=zh-CN&newwindow=1&q=ibus&btnG=Google+搜索&aq=f&oq=" target="_blank">ibus</a>是个不错的选择，我现在就用这个输入法来写这篇文章。  
（3）**点阵字体**  
刚换上系统的时候，总是感觉有点不对劲，看着不舒服。后来才知道ubuntu默认的字体是点阵字体，ubuntu 怎么会默认使用这种字体呢，看起来很费劲，于是马上Google 一下，找到解决方案：  
<span style="color: #ff6600;"> cd /etc/fonts/conf.d<br /> sudo ln -sf ../conf.avail/66-wqy-zenhei-sharp-no13px.conf 66-wqy-zenhei-sharp.conf</span>  
（４）**flash中文乱码**  
这个问题的解决方案：  
<span style="color: #ff6600;"> sudo cp 49-sansserif.conf 49-sansserif.conf_backup <span style="color: #000000;">//备份</span><br /> sudo rm 49-sansserif.conf</span> //删除
