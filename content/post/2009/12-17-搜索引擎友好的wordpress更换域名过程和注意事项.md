---
date: Thu, 17 Dec 2009 09:12:38 +0000
title: 搜索引擎友好的WordPress更换域名过程和注意事项
author: tomheng
layout: post
permalink: /archives/380.html
keywords: '搜索引擎    Wordpress  更换域名 seo 301重定向'
views: 2475
categories:
  - WordPress
tags:
  - DNS
  - SEO
---
<div style="font-size: 12px;">
  <p>
    由于.cn域名的前途堪忧，所以最终决定把我的<a href="http://www.wordpress.org">wordpress</a>博客<a href="http://blog.webfuns.net">趣味互联网</a>的域名从http://blog.webfuns.cn更换成http://blog.webfuns.net。整个过程中的关键就是要保证seo友好，对<a class="wpgallery" title="google 等搜索引擎的结果的相关性与正确性" href="http://blog.webfuns.net/archives/366.html" target="_blank">搜索引擎排名</a>的影响降到最低。因此整个迁移过程比较谨慎，不过还算顺利，现在整理出更换的过程和注意事项。
  </p>
  
  <p>
    要保证seo友好，整个域名迁移过程中最重要的事情就是要做好301重定向。还有就是要保证新域名不能产生死链。
  </p>
  
  <p>
    <strong>准备</strong>
  </p>
  
  <p>
    （1）首先要做的就是把给新域名添加A记录，指向你博客空间的ip地址。<br /> （2）数据备份
  </p>
  
  <p>
    推荐进行数据库的完全备份，这个可以通过<a href="http://www.phpmyadmin.net/home_page/index.php">phpmyadmin</a>实现或者可以通过插件实现。如果不方便进行数据库文件备份，也可以使用wordpress自带的导出功能。如果在更换域名过程中出现以外，这样可以最大限度的保证数据的完整性。
  </p>
  
  <p>
    <strong>博客自身的处理：</strong>
  </p>
  
  <p>
    （1）更改博客的永久链接。
  </p>
  
  <p>
    WordPress后台设置&#8212;->常规
  </p>
  
  <p>
    wordpress安装地址(url)：新域名（<a href="http://blog.webfuns.net/">http://blog.webfuns.net</a>)
  </p>
  
  <p>
    博客地址(url)：新域名（<a href="http://blog.webfuns.net/">http://blog.webfuns.net</a>)
  </p>
  
  <p>
    做好这些设置之后，在固定链接页面中点击更新按钮，这样可以更新博客的.htaccess文件。
  </p>
  
  <p>
    （2）更新博客文章内容的绝对链接
  </p>
  
  <p>
    就是seo中常说的内部链接，更新这些链接为新域名要通过运行sql语句来实现。
  </p>
  
  <p>
    <span style="color: #0000ff;">Update</span> wp_posts <span style="color: #0000ff;">set</span> post_content<span style="color: #0000ff;">=</span><span style="color: #0000ff;">replac<span style="color: #0000ff;">e</span></span><span style="color: #0000ff;">(</span>post_content,&#8217;http://old.com&#8217;,&#8217;http://new.com&#8217;<span style="color: #0000ff;">)</span>
  </p>
  
  <p>
    （3）更新ping链接
  </p>
  
  <p>
    如果你的博客内容之间有链接，wordpress会记录这些链接。在上一步中我们改的只是文章内的这些链接，但是被引用文章的ping链接并没有改变。所以我们还要使用同样的方式去更新这些链接。
  </p>
  
  <p>
    <span style="color: #0000ff;">Update</span> wp_posts <span style="color: #0000ff;">set</span> pinged<span style="color: #0000ff;">=</span><span style="color: #0000ff;">replace</span><span style="color: #0000ff;">(</span>pinged,&#8217;http://old.com&#8217;,&#8217;<a href="http://new.com/">http://new.com</a>&#8216;<span style="color: #0000ff;">)</span>
  </p>
  
  <p>
    <span style="color: #0000ff;"><span style="color: #0000ff;">Update</span><span style="color: #000000;"> wp_comments </span><span style="color: #0000ff;">set</span><span style="color: #000000;"> comment_author_url</span><span style="color: #0000ff;">=</span><span style="color: #0000ff;">replace</span><span style="color: #000000;">(comment_author_url</span><span style="color: #000000;">,&#8217;http://old.com&#8217;,&#8217;</span><a href="http://new.com/"><span style="color: #000000;">http://new.com</span></a><span style="color: #000000;">&#8216;</span><span style="color: #000000;">)</span></span>
  </p>
  
  <p>
    <strong>接下来要做的就是要做301重定向了</strong>
  </p>
  
  <p>
    方法有主要有两种。一种是通过apache的mod_rewrite功能，方法如下：
  </p>
  
  <p>
    <span style="color: #0000ff;">RewriteEngine on</span>
  </p>
  
  <p>
    <span style="color: #0000ff;">RewriteCond %{HTTP_HOST}% ^blog.webfuns.cn(.*)$ [NC]<br /> RewriteRule ^(.*)$ http://blog.webfuns.net/$1 [R=301,L]</span>
  </p>
  
  <p>
    把以上代码写入.htaccess文件上传到wordpress博客根目录下。然后到wordpress后台，在固定链接中点击更新按钮，更新.htaccess把wordpress的重定向链接规则追加到后面。
  </p>
  
  <p>
    第二中是通过php文件来实现：
  </p>
  
  <p>
    <span style="color: #ff0000;"><?php</span>
  </p>
  
  <p>
    <span style="color: #0000ff;">$host</span><span style="color: #0000ff;">=</span><span style="color: #0000ff;">$_SERVER</span>[&#8216;HTTP_HOST&#8217;];
  </p>
  
  <p>
    <span style="color: #0000ff;">i<span style="color: #0000ff;">f</span></span><span style="color: #0000ff;">(</span><span style="color: #0000ff;">$host</span><span style="color: #0000ff;">==</span>&#8216;blog.webfuns.cn&#8217;<span style="color: #0000ff;">)</span><span style="color: #0000ff;">{</span>
  </p>
  
  <p>
    <span style="color: #0000ff;">$uri</span><span style="color: #0000ff;">=</span><span style="color: #0000ff;">$</span>_<span style="color: #0000ff;">SERVER</span><span style="color: #0000ff;">[</span><span style="color: #0000ff;">&#8216;</span>HTTP_URI<span style="color: #0000ff;">&#8216;</span><span style="color: #0000ff;">]</span><span style="color: #0000ff;">;</span>
  </p>
  
  <p>
    <span style="color: #0000ff;">header<span style="color: #0000ff;">(</span></span><span style="color: #0000ff;">“</span>HTTP/1.1  301 move permanetly<span style="color: #0000ff;">”);</span>
  </p>
  
  <p>
    <span style="color: #0000ff;">header(“</span>Location:   http://blog.webfuns.net{$uri}<span style="color: #0000ff;">”);</span>
  </p>
  
  <p>
    <span style="color: #0000ff;">exit;</span>
  </p>
  
  <p>
    <span style="color: #0000ff;">}</span>
  </p>
  
  <p>
    <span style="color: #ff0000;">?></span>
  </p>
  
  <p>
    最后需要做的就是修改你的外链，主要是友情链接和别人引用你博客内容的链接。这个更改比较困难，一般来说更换域名总会丢失一些外部链接，必经更换域名是要付出代价的，所以尽力而为吧，能捡回几个来算几个。
  </p>
  
  <p>
    <span style="color: #ff6600;"><strong>重要提醒：</strong></span>
  </p>
  
  <p>
    上述关于wordpress更换域名的过程和方法适用的场景是新域名和旧域名使用同一个空间。此外域名的更换确实是一件不容易的事情，如非专业人士还是避而远之为妙。当然如果你的网站巨nb，可以无视google，<a class="wpgallery" title="百度" href="http://baidu.com" target="_blank">百度</a>等搜索引擎的排名的话，那就大胆做吧。
  </p>
</div>
