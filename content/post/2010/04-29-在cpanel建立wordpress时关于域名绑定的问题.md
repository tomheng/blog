---
date: Thu, 29 Apr 2010 14:37:08 +0000
title: 在cPanel建立WordPress时关于域名绑定的问题
author: tomheng
layout: post
permalink: /archives/625.html
robotsmeta:
  - index,follow
views: 692
categories:
  - WordPress
---
域名绑定是建立WordPress网站的重要步骤。由于对一些概念理解不到位，有些朋友在这方面遇到了困难，今天把域名绑定的步骤重新说明一下，希望对大家有用。

以用这个域名http://www.yholiday.com，在wpc空间或者其他支持Cpanel的美国空间建立一个WordPress博客为例，

进行说明。

无论是把域名安装在根目录还是子目录都要先对域名进行正确的解析，如果你使用的是wpc的空间，那么请将域名的dns改成两个就可以啦。或者也可以添加A记录，指向到你的空间所在地ip（通过PING命令可以得到你空间的IP，例如ping http://host1.tuikr.com）

建立数据库的步骤就省略了。

**第一种情况：把WordPress博客安装到根目录**

1）把WordPress程序所有文件上传到cpanel空间的根目录一般是public_html

此时public_html目录下应该直接包含WordPress的所有文件

<img src="http://www.chinaz.com/upimg/allimg/100429/1304030.jpg" border="0" alt="" width="299" height="430" />

2）绑定域名

因为我们的WordPress是安装在根目录的，所以域名应该绑定在根目录才可以

在cpanel空间点击附加域（add on domain）菜单，在出现的界面中填入如下内容

<img src="http://www.chinaz.com/upimg/allimg/100429/1304031.jpg" border="0" alt="" width="564" height="295" />

3）如果添加不出错误，那么现在就可以访问你的域名

进行安装啦。

4）安装完成之后，在WordPress后台的设置菜单中把博客的安装地址和博客地址

都改成你的域名：http://www.yholiday.com

**第二种情况：在子目录安装WordPress博客**

1）例如，我们可以在cpanel空间建立如下所示的子目录wp2：

<img src="http://www.chinaz.com/upimg/allimg/100429/1304032.jpg" border="0" alt="" width="302" height="447" />

2）把域名绑定wordpress的安装目录即wp2

<img src="http://www.chinaz.com/upimg/allimg/100429/1304033.jpg" border="0" alt="" width="638" height="317" />

后续步骤同上

说明：示例只是一个简单说明，希望大家认真理解，可以举一反三，多加尝试。只要操作步骤正确，一般是不会遇到问题的。
