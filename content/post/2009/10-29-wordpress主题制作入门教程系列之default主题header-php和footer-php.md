---
date: Thu, 29 Oct 2009 11:59:05 +0000
title: wordpress主题制作入门教程系列（6）之default主题header.php和footer.php
author: tomheng
layout: post
permalink: /archives/205.html
views: 890
categories:
  - WordPress
tags:
  - WordPress 主题
---
<span style="color: #ff00ff;">It is easy ,it is fun!-<span style="color: #000000;">wordpress主题制作入门教程口号</span></span>  
上一讲我们简单<a class="wpgallery" title="wordpress 主题制作教程系列之default主题index.php" href="http://blog.webfuns.net/archives/179.html " target="_blank">分析了index.php文件</a>，并且初步接触了几个模版函数（get\_header() 、get\_footer()、 get_sidebar（））。这一讲我们将简单的看一下header.php、footer.php ，了解几个更有用的函数，做一些数据的简单调用。

（1）**header.php的分析&#8212;&#8212;&#8212;&#8212;bloginfo()模版函数的使用**

<pre lang="php">&gt;

<!--

	#page { background: url("/images/kubrickbg-.jpg") repeat-y top; border: none; }

	#page { background: url("/images/kubrickbgwide.jpg") repeat-y top; border: none; }

-->

&gt;


<div id="page">
  <div id="header">
    <div id="headerimg">
      <h1>
        
      </h1>
      
    </div>
    
  </div>
  
  
  <hr />
  
</div></pre>

<div id="page">
  <p>
    Header.php 和footer.php一样都是WordPress主题中最简单的模版文件了，他们的作用就是显示页面的底部和头部！Html常见的头部信息标签就不说了，主要看一下里面使用的模版函数，首先就是bloginfo()函数，同时我们会发现里面有一个和他长得很像的函数get_bloginfo()函数，那这两个函数什么关系呢？又如何使用呢？
  </p>
  
  <p>
    这两个函数的原型都在wp-includes/general-template.php中！
  </p>
  
  <p>
    function bloginfo($show=&#8221;) {
  </p>
  
  <p>
    echo get_bloginfo($show, &#8216;display&#8217;);
  </p>
  
  <p>
    }
  </p>
  
  <p>
    从bloginfo()的定义中我们知道，bloginfo()函数是调用get_bloginfo()实现的！区别就是bloginfo()把结果直接输出，而get_bloginfo()则是返回一个数据！跟细微的参数差异我们就不说了，他们主要是用来调用WordPress里的关于博客的一些基本信息，这些信息就是在后台设置&#8212;常规选项中设置的一些信息！
  </p>
  
  <p>
    下面列出一些常用的参数列表：
  </p>
  
  <p>
    参数名 说明 返回值举例
  </p>
  
  <table border="1" cellspacing="0" cellpadding="0">
    <tr>
      <td width="189" valign="top">
        <span style="color: #ff6600;">参数名</span>
      </td>
      
      <td width="189" valign="top">
        <span style="color: #ff6600;">说明</span>
      </td>
      
      <td width="189" valign="top">
        <span style="color: #ff6600;">返回值举例</span>
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        html_type
      </td>
      
      <td width="189" valign="top">
        文档类型
      </td>
      
      <td width="189" valign="top">
        Text/html
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        charset
      </td>
      
      <td width="189" valign="top">
        网页编码
      </td>
      
      <td width="189" valign="top">
        Utf8
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        name
      </td>
      
      <td width="189" valign="top">
        博客的名字
      </td>
      
      <td width="189" valign="top">
        趣味互联网
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        stylesheet_url
      </td>
      
      <td width="189" valign="top">
        Style.css 的url地址
      </td>
      
      <td width="189" valign="top">
        http://blog.webfuns.net/wp-content/themes/inove/style.css
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        comments_rss2_url
      </td>
      
      <td width="189" valign="top">
        评论的rss订阅地址
      </td>
      
      <td width="189" valign="top">
        <a href="http://blog.webfuns.net/comments/feed">http://blog.webfuns.net/comments/feed</a>
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        description
      </td>
      
      <td width="189" valign="top">
        副标题
      </td>
      
      <td width="189" valign="top">
        webfuns
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        admin_email
      </td>
      
      <td width="189" valign="top">
        管理员的邮箱
      </td>
      
      <td width="189" valign="top">
        admin@webfuns.cn
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        version
      </td>
      
      <td width="189" valign="top">
        WordPress 的版本
      </td>
      
      <td width="189" valign="top">
        2.8
      </td>
    </tr>
    
    <tr>
      <td width="189" valign="top">
        language
      </td>
      
      <td width="189" valign="top">
        当地的语言
      </td>
      
      <td width="189" valign="top">
        Zh-cn
      </td>
    </tr>
  </table>
  
  <p>
    其他的模版函数就不再解释啦，如果有的朋友感兴趣，可以把函数结果输出到页面看看这些函数究竟是干什么的。比如我们不知道blog_class()是干什么的，就在index.php中加入下列代码：<div style=&#8221;color:red&#8221;><?php blog_class();?></div> 然后打开页面可以看到输出的是：class=&#8221;home blog&#8221; ，就知道他是干什么的啦！
  </p>
  
  <p>
    接下来我们再看一下footer.php，它是用来显示页面底部的信息。这里我重点解释一下如下代码的作用。
  </p>
  
  <pre lang="php">printf(__('%1$s is proudly powered by %2$s', 'kubrick'), get_bloginfo('name'),
		'<a href="http://wordpress.org/">WordPress</a>');</pre>
  
  <p>
    Printf()是php自身的一个函数，学过c的朋友对他肯定很熟悉，在php中的printf()函数和c中的使用方式是一样的。这里做一下简要说明。
  </p>
  
  <p>
    __(&#8216;%1$s is proudly powered by %2$s&#8217;, &#8216;kubrick&#8217;)这一句中的__()也是一个函数（有兴趣的可以搜一下gettext 查看相关内容），这个函数就不解释啦，这里有%1$s %2$s 这里把他们理解成占位符就可以，就是给后面的get_bloginfo(&#8216;name&#8217;)和<a href=&#8221;http://wordpress.org/&#8221;>WordPress</a> 占个位子，预留给他们输出，就是说get_bloginfo(&#8216;name&#8217;)的值将替代%1$s is proudly powered by %2$s&#8217;, &#8216;kubrick中%1$s。
  </p>
  
  <p>
    这样一说大家对这两个文件应该有个大体的了解啦，其实这两个文件的内容都可以重用，一般也不用做多大的改动。所以我们的主题中就直接重用这两个文件。
  </p>
  
  <p>
    有任何问题可以在此留言或者im 我都可以，我会及时做出回复。也欢迎的大家对教程提出意见和建议，我们将做出及时的调整。
  </p>
</div>
