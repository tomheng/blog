---
date: Fri, 03 Jul 2009 12:36:39 +0000
title: php中的字符串（single quote，double quote，heredoc,nowdoc）(二)
author: tomheng
layout: post
permalink: /archives/135.html
views: 1008
categories:
  - LAMP
tags:
  - LAMP
---
在<span><a style="color: #2970a6; text-decoration: none; padding: 0px; margin: 0px;" rel="bookmark" href="http://blog.webfuns.net/archives/129.html">php中的字符串（single quote，double quote，heredoc,nowdoc）(一)</a></span>我们简单总结了php中单引号、双引号、heredoc、nowdoc的用法，这篇文章简单总结一下

**总结说明：**

1)  heredoc 和双引号差不多，只是在deredoc中双引号不用转义

2)  Nowdoc 和单引号的作用差不多，在nowdoc中单引号不用转义

3) Nowdoc 从php 5.3才开始支持。

4) heredoc是输出html的好工具

例如

![][1]

因为在html中可能混有单引号和双引号，这是采用heredoc（或者nowdoc)定义就不用做任何转义，用其他两种方式还需要做转义处理。

5)单引号和双引号在处理字符串中变量的差别

$b=3;

echo $a=&#8221;$b=5&#8243;;//output: 3=5

echo $c=&#8217;$b=5&#8242;;//output: $b=5

在双引号中php会去检查有没有相应的变量，这是php对字符串贪婪模式的体现，所以如果双引号中无变量的时候，最好用单引号，可以节省一下资源，提高效率。

这一点也引出了{}的用途。

示例：

<pre lang="php">$friut='apple';
//我们想要的输出是这样的apples are green;。这只是个示例不一定恰当
echo " $friuts are green;";//output: are green;
echo "{$friut}s are green;"//output: apples are green;</pre>

 [1]: http://blog.webfuns.net/wp-content/uploads/2009/07/code1.JPG
