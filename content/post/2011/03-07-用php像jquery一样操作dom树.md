---
date: Mon, 07 Mar 2011 15:42:48 +0000
title: 用PHP像JQuery一样操作DOM树
author: tomheng
layout: post
permalink: /archives/983.html
robotsmeta:
  - index,follow
views: 1091
categories:
  - LAMP
tags:
  - PHP
---
JQuery对DOM树的操作进行了合理的封装，这样使得操作DOM变得简单起来。最方便的就是JQuery的 选择器，可以帮助我们很方便的找到我们想要的节点。但是在PHP操作DOM树，却不是那么方便，除非你非常精通正则。好吧，问题总有解决的方法。就有高人实现了PHP类似于JQuery的操作方法来操作DOM。

这个项目的名字叫：PHP Simple HTML DOM Parser

**项目主页**：<http://simplehtmldom.sourceforge.net/>

**特点**：

  * A HTML DOM parser written in PHP5+ let you manipulate HTML in a very easy way!
  * Require **PHP 5+**.
  * Supports invalid HTML.
  * Find tags on an HTML page with selectors just like [jQuery][1].
  * Extract contents from HTML in a single line

**示例**：

<span style="color: #0000ff;">$html=str_get_html(&#8220;<p class=&#8217;article&#8217;>hello everyone</p>&#8221;);</span>

<span style="color: #0000ff;">$html->find(&#8220;p.article&#8221;);</span>

<span style="color: #339966;">有了这个扩展，就可以很轻松的在PHP中操作DOM树了。</span>

 [1]: http://jquery.com/
