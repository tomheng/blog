---
date: Mon, 19 Oct 2009 11:07:30 +0000
title: wordpress主题制作入门教程系列(3)之default主题的组织结构
author: tomheng
layout: post
permalink: /archives/174.html
views: 796
categories:
  - WordPress
tags:
  - WordPress
---
如果你不明白我们在做什么，请回顾我们wordpress主题制作入门教程系列上一讲：<a class="wpgallery" title="wordpress主题制作入门教程系列(2)之主题制作基本流程" href="http://blog.webfuns.net/archives/171.html" target="_blank">wordpress主题制作入门教程系列(2)之主题制作基本流程</a>

一、主要分为三块：

(1)Images目录 存放主题中所使用的图片

(2)Css 样式文件 style.css rtl.css

(3)模版文件 这个是我们以后分析的重点！

<div id="attachment_175" style="width: 289px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-175" href="http://blog.webfuns.net/archives/174.html/mulu"><img class="size-full wp-image-175" title="mulu" src="http://blog.webfuns.net/wp-content/uploads/2009/10/mulu.JPG" alt="default主题的文件组织结构" width="279" height="256" /></a>
  
  <p class="wp-caption-text">
    default主题的文件组织结构
  </p>
</div>

<div id="attachment_176" style="width: 714px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-176" href="http://blog.webfuns.net/archives/174.html/wordpress-php"><img class="size-full wp-image-176 " title="wordpress-php" src="http://blog.webfuns.net/wp-content/uploads/2009/10/wordpress-php.jpg" alt="default主题中各文件的关系图" width="704" height="561" /></a>
  
  <p class="wp-caption-text">
    default主题中各文件的关系图
  </p>
</div>

这个图片说明了这些文件的重要性，从中我们可以看出index.php是最重要的一个文件。其次是single.php 、page.php、search.php、  arcchive.php、 404.php 文件，他们在WordPress主题中各司其职，分工写作来构建一个完美的WordPress主题

（2）style.css 样式文件

<span style="color: #0000ff;">/*</span>

<span style="color: #0000ff;">Theme Name: <span style="color: #ff0000;">WordPress Default</span></span>

<span style="color: #0000ff;">Theme URI: <span style="color: #ff0000;">http://wordpress.org/</span></span>

<span style="color: #0000ff;">Description: <span style="color: #ff0000;">The default WordPress theme based on the famous <a href=&#8221;http://binarybonsai.com/kubrick/&#8221;>Kubrick</a>.</span></span>

<span style="color: #0000ff;">Version: <span style="color: #ff0000;">1.6</span></span>

<span style="color: #0000ff;">Author: <span style="color: #ff0000;">Michael Heilemann</span></span>

<span style="color: #0000ff;">Author URI: <span style="color: #ff0000;">http://binarybonsai.com/</span></span>

<span style="color: #0000ff;">Tags: <span style="color: #ff0000;">blue, custom header, fixed width, two columns, widgets</span></span>

<span style="color: #0000ff;">Text Domain: <span style="color: #ff0000;">kubrick</span></span>

<span style="color: #0000ff;"> </span>

<span style="color: #ff0000;">Kubrick v1.5</span>

<span style="color: #ff0000;">http://binarybonsai.com/kubrick/</span>

<span style="color: #ff0000;"> </span>

<span style="color: #ff0000;">This theme was designed and built by Michael Heilemann,</span>

<span style="color: #ff0000;">whose blog you will find at http://binarybonsai.com/</span>

<span style="color: #0000ff;"> </span>

<span style="color: #ff0000;">The CSS, XHTML and design is released under GPL:</span>

<span style="color: #ff0000;">http://www.opensource.org/licenses/gpl-license.php</span>

<span style="color: #0000ff;"> </span>

<span style="color: #0000ff;">*/</span>

style.css文件的开头就是一段css注释，他的作用就是对制作的主题的简单说明。这里包含了一些与主题的相关信息（Theme Name、Author等），只要按照上面的格式填写就可以在安装主题的预览页面看到相关的信息。如果制作新的主题，这里可以填写上自己主题的信息。上面红色的部分是根据自己的需要进行填写的，前面蓝色的部分不用动。

今天就先这一讲就说这些吧。有任何问题可以在此留言或者im 我都可以！我会及时做出回复！也欢迎的大家对教程提出意见和建议，我们将做出及时的调整！
