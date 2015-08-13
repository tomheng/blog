---
date: Sun, 12 Dec 2010 13:19:10 +0000
title: SAE上的wordpress rewrite规则
author: tomheng
layout: post
permalink: /archives/921.html
robotsmeta:
  - index,follow
views: 1466
categories:
  - WordPress
tags:
  - SAE
---
如果你有一个[SAE][1]的账号，那么可以在上面搭建一个[wordpress][2]博客,搭建过程很简单。  
[ ][3]

[wp4sae][3]下载和安装说明 http://code.google.com/p/wp4sae/

安装之后有个问题，SAE是nigix的而worpress的url重写规则是基于apache的，所以直接把.htaccess放在根目录是不起作用的。

所以我们需要一个可以在SAE上的规则使得wordpress可以正常运行。

规则如下：

<div id="_mcePaste">
  <span style="color: #0000ff;">&#8212;</span>
</div>

<div id="_mcePaste">
  <span style="color: #0000ff;">name: seo123</span>
</div>

<div id="_mcePaste">
  <span style="color: #0000ff;">version: 2</span>
</div>

<div id="_mcePaste">
  <span style="color: #0000ff;">handle:</span>
</div>

<div id="_mcePaste">
  <span style="color: #0000ff;">&#8211; rewrite: if(!is_file()&&!is_dir()) goto &#8220;index.php?%{QUERY_STRING}&#8221;</span>
</div>

**说明：**

seo123是SAE项目的名字

version是活动版本

这两项都是SAE自己生产的，不用改动，关键是第三项：

&#8211; rewrite: if(!is\_file()&&!is\_dir()) goto &#8220;index.php?%{QUERY_STRING}&#8221;

这个就是对应的规则，注意这一行的<span style="color: #ff0000;">开头必须有两个空格</span>，

 [1]: http://sae.sina.com.cn/
 [2]: http://www.wordpress.org
 [3]: http://code.google.com/p/wp4sae/
