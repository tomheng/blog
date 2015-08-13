---
date: Tue, 06 Oct 2009 12:24:23 +0000
title: wordpress主题制作入门教程系列之主题制作基本流程 (worpress教程网）
author: tomheng
layout: post
permalink: /archives/187.html
views: 536
categories:
  - WordPress
tags:
  - WordPress
  - wordpress theme
---
原文链接：wordpress主题制作入门教程系列之主题制作基本流程

<span style="color: #ff00ff">Is is easy ,it is fun.</span>没错制作主题教程是简单的也是有趣的。上一讲我们简单的做了个说明，现在就让我们先来配置一下我们的开发环境吧。

### 主题制作基本流程

(1)**在自己的电脑安装WordPress运行的环境**（<a class="cflabel" title="WordPress入门视频教程2 在本机搭建WordPress的安装环境" href="http://www.wpcourse.com/wordpress-lesson2-setup-wordpress-environment-in-localhost.html" target="_blank">在本机搭建WordPress的安装环境XAMPP</a>===<a class="cflabel" title="WordPress教程总目录" href="http://www.wpcourse.com/menu" target="_blank">WordPress教程网教程系列</a> ）；

安装过程中可能会遇到一些问题，一般就是迅雷等如见占用80端口。所以安装之前最好把迅雷给关了。另外phpnow ,appserve ,nertrigo等都可以完成xampp的工作。

(2)**在 本机服务器安装WordPress**（<a class="cflabel" title="WordPress入门视频教程2 在本机搭建WordPress的安装环境" href="http://www.wpcourse.com/wordpress-lesson3-install-wordpress-in-localhost.html" target="_blank"> 在本机安装WordPress全过程</a> ===<a title="WordPress教程总目录" href="http://www.wpcourse.com/menu" target="_blank">WordPress教程网教程系列</a> ）。

这个相对简单一些，如果一切顺利这时侯，在自己的浏览器输入http://localhost就可以看到自己装的WordPress啦。

(3)**推荐：**准备一些软件，dreamwerver ，fireworks,最好装两个以上的浏览器（包含ie,fireworks)当然这些不一定用的着，只是推荐而已，大家根据自己的知识水平准备一下就可以啦。

### 现在万事俱备，接下来就开始介绍教程制作的相关知识。

(1)**主题的存放位置**。

所有的主题都存在于：你的博客所在目录/wp-content/themes下，每一个文件夹就是一个主题。所以我们制作主题的第一步就是在这个目录建立一个文件夹，文件夹的名字可以用主题的名字，您还得先为主题取个好听的名字奥。比如我们就建立一个叫wpc-tomheng的文件夹。

到wordpress后台看一下是不是存在我们的主题。令人失望的是没有看到我们的主题，这是什么问题呢？其实建立一个文件夹只是给我们的主题买了块地皮而已，我们还需要在上面建立一些东西，才能在后台看到的。要在后台看到我们的主题最主要的一步就是在主题目录（我这里是wpc-tomheng,要根据自己的具体情况在相应的目录里，以后我们就直接以wpc-tomheng作为说明，您在操作的时候要具体转化为自己的目录。）建立一个叫style.css的文件。

这个文件是用来存放css样式文件，这里面也包含了关于主题的一些信息。与主题相关的信息都是放在/\*主题相关的信息\*/里面。我们在里面写入最简单的一个信息-关于主题的名称。在style.css文件里写入如下信息

<span style="color: #0000ff">/*</span>

<span style="color: #0000ff">Theme Name: wpc-tomheng</span>

<span style="color: #0000ff">*/</span>

<span style="color: #ff00ff">说明</span>：在这里可以填入更多的相关信息，但是都要符合<a class="cflabel" href="http://codex.wordpress.org/Theme_Development" target="_blank">WordPress的规范</a>才行这些信息不是必要，但是我们最好填写一些必备的信息（如：主题的名字，作者，等信息），这样看起来更规范。保存文件。到后台看一下，我们依然没有发现我们的主题，我们的主题却被列入了已损坏的主题里面，提示信息为：缺失模版文件。是的我们的主题确实小但是五脏并未俱全，接下来再建立一个模版文件就好啦。在wpc-tomheng 目录建立一个index.php的文件，这时候我们的主题能在后台的主题选项目录看到，同时我们的主题也可以安装了。

这就是一个新的主题必须具有的最基本的结构（style.css 和index.php文件)。现在我们安装上自己制作的主题，但是我们到前台查看自己的博客时候，什么内容也没有看到。这是为什么呢？

当然，我们的工作刚刚起步。我们还需要给我们的主题继续的添加东西，才可以在前台有个完美的展示。在index.php里写入任何的html标签或者简单的文本都可以在前台显示出来。You may wan to try it .

但是我们怎么把我们在博客里的文章或者其他的信息显示出来呢，这就要靠我们的WordPress template tags (http://codex.wordpress.org/Template_Tags )。我们在这里先介绍一个最简单的tag，（在开始的几讲里我们只是介绍一下主题的最基本知识和相关的流程，关于更详细的教程我们会在后续的章节发布出来，请密切关注wpc的动向）。

在index.php里写入如下代码：

<html>

<body>

<?php

bloginfo(&#8216;name&#8217;);

?>

</body>

</html>

现在在博客的首页应该可以看到自己博客的名字了，这就是tag的一个最简单的应用。

现在wp-tomehng 应该算是一个完整的主题，但绝不是一个完美的主题。因为他基本没有展现我们博客的内容，也没有漂亮的外观。展现博客的内容要靠template tags（例如bloginfo())来完成,改变外观要靠style.css来完成。这也是制作主题的关键，最能展示个人创意的地方。

接下来，如果您愿意把自己制作的主题release出来的话，就可以打包成.zip文件，然后提交到WordPress。

恩，现在是不是对WordPress主题制作有个基本的了解了，接下来我们会详细的讲解template tags的相关的知识。

有任何问题可以在此留言或者im 我都可以！我会及时做出回复！也欢迎的大家对教程提出意见和建议，我们将做出及时的调整！
