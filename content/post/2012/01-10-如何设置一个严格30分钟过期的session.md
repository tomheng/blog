---
date: Tue, 10 Jan 2012 15:35:45 +0000
title: 如何设置一个严格30分钟过期的Session
author: tomheng
layout: post
permalink: /archives/1329.html
views: 548
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - PHP基础
---
问题是[Laruence][1]在微博上提出的:

> 我在面试的时候, 经常会问一个问题: “如何设置一个30分钟过期的Session?”, 大家不要觉得看似简单, 这里面包含的知识多着呢, 特别适合考察基本功是否扎实, 谁来回答试试? 呵呵

说实话，如果是面试我的话，我回答不好。我找了一下stackoverflow发现有人讨论过同样的问题，[How do I expire a PHP session after 30 minutes?][2]，看了一下[Gumbo][3]回复应该是最好的。过了不久Laruence也整理了一篇[文章][4]，讲的更全面一些。 Gumbo说的对，要回到好这个问题只要弄清楚**[session.gc_maxlifetime][5]**和****[session.cookie_lifetime][6]****这两个配置的原理就可以了。 **session.gc_maxlifetime**这个时间是指的Session过多少时间会被session GC回收。这里需要主要以下几点

  * GC启动本身是有概率的（ [session.gc_probability][7]和[session.gc_divisor][8]）
  * 不同的脚本具有不同的 session.gc\_maxlifetime 数值但是共享了同一个地方存储会话数据，则具有最小数值的脚本会清理数据。此情况下，与 session.save\_path 一起使用本指令
  * GC和这个时间比较的时候是用的sesssion文件的mtime（ >PHP 4.2.3）而不是atime

**session.cookie_lifetime**

这个是Server发送给浏览器的cookie过期时间

所以只要明白了这两个配置的原理，那么这个问题至少会回答及格（我认为）。

所以手册很重要，不管是哪种技术，如果把手册研究一篇，搞懂80%以上，那么不是大牛也是个高级工程师鸟。

&nbsp;

 [1]: http://t.sina.com.cn/laruence
 [2]: http://stackoverflow.com/questions/520237/how-do-i-expire-a-php-session-after-30-minutes
 [3]: http://stackoverflow.com/users/53114/gumbo
 [4]: http://www.laruence.com/2012/01/10/2469.html
 [5]: http://php.net/manual/en/session.configuration.php#ini.session.gc-maxlifetime
 [6]: http://php.net/manual/en/session.configuration.php#ini.session.cookie-lifetime
 [7]: http://www.php.net/manual/en/session.configuration.php#ini.session.gc-probability
 [8]: http://www.php.net/manual/en/session.configuration.php#ini.session.gc-divisor
