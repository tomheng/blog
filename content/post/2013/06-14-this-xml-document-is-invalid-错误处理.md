---
date: Fri, 14 Jun 2013 13:24:13 +0000
title: This XML document is invalid 错误处理
author: tomheng
layout: post
permalink: /archives/1681.html
views: 244
categories:
  - LAMP
tags:
  - PHP基础
  - XML
---
在做XML解析的时候很可能遇到This XML document is invalid这种错误。

在PHP中根据使用的解析函数（字符串编码为UTF-8）不同可能的提示有以下两种：

1）xml_parser*函数错误提示

This XML document is invalid, likely due to invalid characters. XML error: Invalid character at line 30, column 25

2）XMLReader类 或 simplexml_load*函数

error on line 30 at column 25: Input is not proper UTF-8, indicate encoding !  
Bytes: 0x1D 0xE7 0xBB 0x

搜索了好久没有找到完美的解决方案，最后分析发现出现这种问题一般是因为出现了不可见字符。于是就试了试能不能通过去除不可见字符的方式来绕过这个错误提示，从而使解析继续下去。经过实验在我的测试中是可以的，所以分享出来，希望对遇到同样问题的朋友能节约一些时间。

可以通过如下方式去除字符串中的不可见字符。

preg\_replace(&#8216;/[^\P{C}\n]+/u&#8217;, &#8221;, $utf8\_data）
