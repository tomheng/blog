---
date: Thu, 26 Nov 2009 03:12:16 +0000
title: wordpress插件Ecall-js方式外部调用wordpress文章
author: tomheng
layout: post
permalink: /archives/313.html
views: 2975
categories:
  - My project
tags:
  - JavaScript
  - 插件
---
在使用<a title="wordpress" href="http://wordpress.org" target="_blank">wordpress</a>的过程中我也曾想wordpress能不能像discuzz或者<a title="phpwind" href="http://www.phpwind.com" target="_blank">phpwind</a>一样用js的方式调用博客的文章，<a class="wpgallery" title="wordpress教程网" href="http://www.wpcourse.com" target="_blank">wodpress教程网</a>的用户也有问过这个问题，wordpress插件Ecall就是为了解决这个问题而写的，<span style="background-color: #ffffff;">有了这个插件你就可以从任何一个站点调用定制的wordpress文章内容了。</span>

<span style="color: #ff6600;">下载</span>：<a class="wpgallery" title="wordpress 插件ecall" href="http://wordpress.org/extend/plugins/ecall/" target="_blank">Ecall</a>

**wordpress外部调用插件Ecall更新日志**

  1. <a style="color: #2970a6; text-decoration: none; padding: 0px; margin: 0px;" rel="bookmark" href="http://blog.webfuns.net/archives/358.html"><strong>WordPress外部调用插件（js方式）-Ecall插件更新至1.12.15</strong></a>
  2. **<a style="color: #2970a6; text-decoration: underline; padding: 0px; margin: 0px;" title="js方式外部调用wordpress文章插件Ecall更新1.12.7版本" href="http://blog.webfuns.net/archives/341.html">js方式外部调用wordpress文章插件Ecall更新1.12.7版本</a>**
  3. **<a style="color: #2970a6; text-decoration: underline; padding: 0px; margin: 0px;" title="js方式外部调用wordpress文章插件Ecall更新1.11.27版本" href="http://blog.webfuns.net/archives/327.html">js方式外部调用wordpress文章插件Ecall更新1.11.27版本</a>**

**wordpress插件Ecall的主要特点：**

<span style="background-color: #ffffff;">（1）自由定制模版</span>

<span style="background-color: #ffffff;">（2）缓存加速</span>

<span style="background-color: #ffffff;">（3）隐藏分类</span>

<span style="background-color: #ffffff;">（4）授权机制，防止恶意调用</span>

<span style="background-color: #ffffff;">（5）文章的js调用方式</span>

(6)<a class="wp-oembed" title="WordPress插件开发建议" href="http://blog.webfuns.net/archives/392.html" target="_blank">遵循WordPress插件开发规范</a>

**wordpress插件Ecall使用说明：**

(1)你可以在任意网站加入如下代码：

<script type=&#8217;text/javascript&#8217; src=&#8221;http://wpcourse.com/api.php?key=123&cid=1&rows=2&#8243;></script>

http://wpcourse.com:代表博客域名

key：代表插件生成的授权密钥（在插件管理页面）

cid:代表目录的id

rows:代表显示的数据调试 可选参数

**wordpress插件Ecall注意事项**

(1)可能由于某种原因，在网站根目录没有api.php文件，你可以手动到插件安装的目录把api_temp.php复制到根目录，然后重命名为api.php

在1.12.15以后的版本已经不再使用api.php文件

/\*&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-分割线&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;\*/

**ps:**

（1）由于插件刚刚发布，存在很多欠缺的地方，大家如果有什么问题和建议可以联系<a title="tomheng" href="http://blog.webfuns.net/about-tomheng-profile" target="_blank">tomheng</a>。

（2）在插件开发中也遇到了很多问题，主要是svn不是很熟悉，版本没有控制好。前100多个下载的用户可能都是有问题的版本，实在是对不起大家了。

（3）如果你觉着这个插件有用并且值得继续维护下去，可以通过捐款的方式来支持插件的开发。

<p style="line-height: 39px; height: 39px;">
  （支付宝和paypal)帐号：<a rel="attachment wp-att-315" href="http://blog.webfuns.net/archives/313.html/donate-tomheng-2"><img class="size-full wp-image-315 alignnone" title="donate-tomheng" src="http://blog.webfuns.net/wp-content/uploads/2009/11/donate-tomheng1.jpg" alt="donate-tomheng" width="264" height="39" /></a>
</p>

<a rel="attachment wp-att-315" href="http://blog.webfuns.net/archives/313.html/donate-tomheng-2"> </a>

<a rel="attachment wp-att-315" href="http://blog.webfuns.net/archives/313.html/donate-tomheng-2"><span style="background-color: #ffffff;"><br /> </span></a>

<a rel="attachment wp-att-315" href="http://blog.webfuns.net/archives/313.html/donate-tomheng-2"> </a>
