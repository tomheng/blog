---
date: Thu, 05 Jan 2012 12:38:06 +0000
title: MySQL中得到当前使用的数据库
author: tomheng
layout: post
permalink: /archives/1320.html
views: 351
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - MySQL
---
在MySQL中用use 切换使用的数据库（或者在PHP中用mysql\_select\_db函数)，反过来我们怎样知道正在使用的是哪个数据库呢？

这种需求很少遇到，但是还是会遇到的：）

所以找了一下，发现MySQL可以通过<span style="color: #0000ff;">select databse()</span>命令来查看当前使用的数据库。

ps:[MySQL database函数][1]

推荐：[Stop Making Apps][2]

再搭车推荐：

*《他们在毕业的前一天爆炸 》《听说》《那些年，我们一起追的女孩》值得一看*

 [1]: http://dev.mysql.com/doc/refman/5.0/en/information-functions.html#function_database
 [2]: http://techcrunch.com/2011/11/11/start-making-sense/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+Techcrunch+%28TechCrunch%29
