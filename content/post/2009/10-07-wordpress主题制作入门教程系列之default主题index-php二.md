---
date: Wed, 07 Oct 2009 12:07:54 +0000
title: wordpress主题制作入门教程系列（5）之default主题index.php(二)
author: tomheng
layout: post
permalink: /archives/208.html
robotsmeta:
  - index,follow
views: 485
categories:
  - WordPress
tags:
  - WordPress
---
<span style="color: #ff00ff;">It is easy ,it is fun!-<span style="color: #000000;">wordpress主题制作入门教程口号。</span></span>

<span style="color: #ff00ff;"><span style="color: #000000;">上一讲：<a class="wpgallery" title="wordpress 主题制作教程系列（4）之default主题index.php" href=" http://blog.webfuns.net/archives/179.html" target="_blank">wordpress 主题制作教程系列（4）之default主题index.php</a></span></span>

通过上一讲的说明，大家应该对bloginfo()函数运用自如了吧！这个函数在footer.php和header.php中扮演着重要的角色。那么我们今天的任务将回归index.php这个文件，把剩下的内容讲完。

我们在第二讲里已经涉及到过index.php，但我们只是说了一下模版加载函数（get_*()函数）。对于大段的代码，我们并没有作分析，其实他们才是index.php的重头戏呢！今天我们就来看一下那些代码究竟是干什么的。

<a rel="attachment wp-att-209" href="http://blog.webfuns.net/archives/208.html/ind"><img class="aligncenter size-medium wp-image-209" title="ind" src="http://blog.webfuns.net/wp-content/uploads/2009/10/ind-300x193.jpg" alt="ind" width="300" height="193" /></a>

从图中我们可以看出剩余的代码主要的作用就是调用了文章的数据。

代码：

<pre class="brush:php ;first-line:1"><div id="content" class="narrowcolumn">
  <div>
    id="post-"&gt;
    
    <small> <!-- by  --></small>
    
    |
  </div>
  
</div></pre>

**下面简单介绍一下，这里用到得几个重要的函数。**

have_posts() 判断是否有文章发表 返回一个Boolean值

the_post() 取得当前文章的数据

the_ID() 打印当前文章的id

the_permalink() 输出文章的永久链接

the_title() 输出文章的标题

the_author() 输出文章的作者

the_content 输出文章的内容

the_tags 输出标签

get\_the\_category_list(&#8216;, &#8216;) 的分类列表数据

edit\_post\_link 编辑链接

comments\_popup\_link 评论链接

previous\_posts\_link 前一篇文章的链接

next\_posts\_link 下一篇文章的链接

这几个函数都很简单，可以直接拿过来用，可以用在任何地方，很灵活的使用它们，实现个性化的主题，这就是WordPress的魔力所在。当然这些函数里有些需要一些具体的参数，不过这些参数都是可选的，初学的话，可以不加这些参数。

有任何问题可以在此留言或者im 我都可以，我会及时做出回复！也欢迎的大家对教程提出意见和建议，我们将做出及时的调整。
