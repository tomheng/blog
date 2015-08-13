---
date: Mon, 16 Jan 2012 14:02:08 +0000
title: DNS预获取（dns-prefetch）
author: tomheng
layout: post
permalink: /archives/1357.html
views: 5001
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - DNS
---
今天翻看twitter的源码的时候看到了一下内容：

<span style="color: #0000ff;"><link rel=&#8221;dns-prefetch&#8221; href=&#8221;http://a0.twimg.com&#8221;/></span>

<span style="color: #0000ff;"><link rel=&#8221;dns-prefetch&#8221; href=&#8221;http://a1.twimg.com&#8221;/></span>

<span style="color: #0000ff;"><link rel=&#8221;dns-prefetch&#8221; href=&#8221;http://a2.twimg.com&#8221;/></span>

<span style="color: #0000ff;"><link rel=&#8221;dns-prefetch&#8221; href=&#8221;http://a3.twimg.com&#8221;/></span>

<span style="color: #0000ff;"><link rel=&#8221;dns-prefetch&#8221; href=&#8221;http://api.twitter.com&#8221;/></span>

查阅了相关资料，知道DNS Prefetch也就是DNS预获取，也是前段优化的一部分。在前段优化中关于DNS的有两点：一是减少DNS的请求次数，第二个就是进行DNS预先获取。

DNS Prefetch 已经被下面的浏览器支持

  * Firefox: 3.5+
  * Chrome: Supported
  * Safari 5+
  * Opera: Unknown
  * IE: 9 (called &#8220;Pre-resolution&#8221; on blogs.msdn.com)

默认情况下浏览器会对页面中和当前域名（正在浏览网页的域名）不在同一个域的域名进行预获取，并且缓存结果，这就是隐式的DNS Prefetch。如果想对页面中没有出现的域进行预获取，那么就要使用显示的DNS　Prefetch了，也就是使用link标签：

<span style="color: #0000ff;"><link rel=&#8221;dns-prefetch&#8221; href=&#8221;http://api.twitter.com&#8221;/></span>

DNS Prefetch应该尽量的放在网页的前面，推荐放在<meta charset=&#8221;/>后面。

PS:可以通过下面的标签禁止隐式的DNS Prefetch。  
<span style="color: #0000ff;"><meta http-equiv=&#8221;x-dns-prefetch-control&#8221; content=&#8221;off&#8221;></span>

内容整理自：[DNS-Prefetching][1]

参考：  
[网站优化应重视 DNS 预获取(DNS Prefetching)][2]

 [1]: https://github.com/h5bp/html5-boilerplate/wiki/DNS-Prefetching
 [2]: http://www.dbanotes.net/web/dns_prefetching.html
