---
date: Mon, 01 Nov 2010 15:10:20 +0000
title: discuz x1.5后台无法登陆问题解决方法？
author: tomheng
layout: post
permalink: /archives/883.html
keywords: discuz x1.5后台无法登陆问题
robotsmeta:
  - index,follow
views: 6362
categories:
  - LAMP
tags:
  - discuz x1.5
---
今天[无限俱乐部论坛][1]管理员后台突然无法登陆了，表现为反复的让我输入密码，但是没有反应。搜索了很长时间也没有找到解决方案，有的说是IP登陆限制造成的，建议修改/bbs/config/config\_global\_default.php 中的

<span style="color: #0000ff;">$_config[&#8216;admincp&#8217;][&#8216;checkip&#8217;] = 0</span>; //<span style="color: #008080;"> 后台管理操作是否验证管理员的 IP, 1=是[安全], 0=否。仅在管理员无法登陆后台时设置 0</span>

可是我修改后依然无法登陆后台，显然我这种情况不是由于IP限制造成的。

于是乎开始翻看discuz x1.5的源码，调试n次后发现问题在于ucenter的连接方式出了问题。原先是用的数据库方式，昨天晚上我改成了接口方式，嗯问题就出在这里了。

知道了问题的原因就好办了，我们可以在/bbs/config/config_ucenter.php中修改<span style="color: #0000ff;">define(&#8216;UC_CONNECT&#8217;, &#8221;);</span>为<span style="color: #ff0000;">define(&#8216;UC_CONNECT&#8217;, &#8216;mysql&#8217;);</span>

修改完成后就可以顺利登陆 x1.5的后台了。

<span style="color: #ff0000;">PS:今天的额外收获，如果无法登陆后，可以先修改后台的登陆密码次数限制，在/bbs/source/function/function_member.php中查找logincheck()函数，这个函数就是返回剩余的登陆次数，所以你可以在return语句前面加上$return=10;</span>

 [1]: http://bbs.wuxianle.com
