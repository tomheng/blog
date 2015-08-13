---
date: Mon, 11 Jan 2010 11:53:08 +0000
title: DOCTYPE怎么支持html5
author: tomheng
layout: post
permalink: /archives/437.html
views: 622
categories:
  - web-trend
tags:
  - HTML5
---
<a href="http://en.wikipedia.org/wiki/Document_Type_Declaration" target="_blank">DOCTYPE</a>即DTD，文档类型定义。通常来说他决定了浏览器选用什么样的布局模式来显示网页。选用不同的**DOCTYPE**通常会对网页的布局和书写规则产生影响。现在比较常用的XHTML 1.0的DOCTYPE主要有三种。

1）**过度类型**

<table style="background: #c2c7ce;" border="0" cellspacing="0" cellpadding="2" width="577">
  <tr>
    <td width="575" valign="top">
      <span style="color: #0000ff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span>
    </td>
  </tr>
</table>

这是一种不是很严格的DOCTYPE,一些不被推荐的标签也可以使用。现在大多数的网页都在使用这种类型。

2）严格类型

<table style="background: #c2c7ce;" border="0" cellspacing="0" cellpadding="2" width="577">
  <tr>
    <td width="575" valign="top">
      <span style="color: #0000ff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd&#8221;></span>
    </td>
  </tr>
</table>

这种要求比较严格不允许使用表现性的标签，如<hr/>。标签的书写必须符合规定。

3）框架类型

<table style="background: #c2c7ce;" border="0" cellspacing="0" cellpadding="2" width="577">
  <tr>
    <td width="575" valign="top">
      <span style="color: #0000ff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Frameset//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd&#8221;></span>
    </td>
  </tr>
</table>

一种支持框架的DOCTYPE。

此外还有支持XHML4.01的DTD和适合手机浏览器的DTD，参见<a href="http://en.wikipedia.org/wiki/Document_Type_Declaration" target="_blank">维基百科的DOCTYPE的介绍</a>。

那么如何让我们的网站支持仍处于开发阶段的HTML5呢？方法很简单，使用如下的类型，那么我们的网站就可以支持<a href="http://blog.webfuns.net/archives/352.html" target="_blank">HTML5</a>啦，同时向后兼容。

<table style="background: #86d1f8;" border="0" cellspacing="0" cellpadding="2" width="400">
  <tr>
    <td width="400" valign="top">
      <strong><!DOCTYPE HTML></strong>
    </td>
  </tr>
</table>

就这么简单网站就可以支持HTML5啦，很简单的书写方式，相对于老式的DOCTYPE，这个更容易记忆。关键是这样的类型我们现在就可以使用，如此短的定义正合Google这种大流量网站的

心意。除了Google以为，现在使用这种类型的网站还有<a href="http://www.taobao.com" target="_blank">淘宝</a>。
