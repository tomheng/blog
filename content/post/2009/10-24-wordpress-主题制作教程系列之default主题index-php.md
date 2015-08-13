---
date: Sat, 24 Oct 2009 08:49:56 +0000
title: wordpress 主题制作教程系列（4）之default主题index.php
author: tomheng
layout: post
permalink: /archives/179.html
robotsmeta:
  - index,follow
views: 698
categories:
  - WordPress
tags:
  - WordPress
  - wordpress theme
---
<span style="color: #ff00ff;">Ii is easy,it is fun.</span>这是我们wordpress 主题制作教程的口号，事实就是这样的，慢慢体会吧。

在我们“<span style="color: #0000ff;"><a class="wpgallery" title="wordpress主题制作入门教程系列(3)之default主题的组织结构" href="http://blog.webfuns.net/archives/174.html" target="_blank">教程的wordpress主题制作入门教程系列(3)之default主题的组织结构</a>”</span>这一讲中对教程做了概要说明，并且简要分析了style.css文件。这一讲我们主要分析一下index.php和header.php ，index.php是一个很重要的文件，通过一讲的内容不能够讲明白，我打算在本讲中将一部分内容剩余的内容在剩余的教程中穿插讲解。

default 主题分析 （index.php )

打开index.php文件，我们看到如下代码。不要头大，这些代码其实很简单，庖丁解牛，慢慢搞定它。

<pre class="brush:php html;first-line:1">/**

<?php
/**
 * @package WordPress
 * @subpackage Default_Theme
 */

get_header(); ?>

	

<div id="content" class="narrowcolumn" role="main">
  <?php //include('g.php');?>
  	
  
  <?php if (have_posts()) : ?>
  
  		
  
  <?php while (have_posts()) : the_post(); ?>
  
  			&lt;div 
  
  <?php post_class() ?> id="post-
  
  <?php the_ID(); ?>">
  				
  
  <h2>
    <a href="<?php the_permalink() ?>" rel="bookmark" title="Permanent Link to <?php the_title_attribute(); ?>"><?php the_title(); ?></a>
  </h2>
  				
  
  <small><?php the_time('F jS, Y') ?> 
  
  <!-- by <?php the_author() ?> --></small>
  
  				
  
  <div class="entry">
    <?php the_content('Read the rest of this entry &raquo;'); ?>
    				
  </div>
  
  				
  
  <p class="postmetadata">
    <?php the_tags('Tags: ', ', ', '<br />'); ?> Posted in 
    
    <?php the_category(', ') ?> | 
    
    <?php edit_post_link('Edit', '', ' | '); ?>  
    
    <?php comments_popup_link('No Comments &#187;', '1 Comment &#187;', '% Comments &#187;'); ?>
  </p>
  			
</div>

		

<?php endwhile; ?>

		

<div class="navigation">
  <div class="alignleft">
    <?php next_posts_link('&laquo; Older Entries') ?>
  </div>
  			
  
  <div class="alignright">
    <?php previous_posts_link('Newer Entries &raquo;') ?>
  </div>
  		
</div>

	

<?php else : ?>

		

<h2 class="center">
  Not Found
</h2>
		

<p class="center">
  Sorry, but you are looking for something that isn't here.
</p>
		

<?php get_search_form(); ?>

	

<?php endif; ?>

	&lt;/div>



<?php get_sidebar(); ?>



<?php get_footer(); ?>
</pre>

我们把这一段冗长的代码分成四部分来逐个分析，也许这样会更加容易理解一些。

第一部分：

<?php

/**

* @package WordPress

* @subpackage Default_Theme

*/

get_header();

?>

这一段代码相对较简单一些，包括注释和一个函数get\_header() （这个函数是一个典型是一个wp 模版函数）。他的作用是加载头部文件即header.php，相当于php中的include 函数的作用。这个函数的原型在wp-includes/general-template.php，有一定php基础的朋友可以看一下函数的具体定义实现方法，第三部分（）get\_sitebar()函数和第四部分（）get\_footer()，实现的原理和get\_header()一样，他们的作用通过他们的名字就可一猜出来的。

剩下代码就是第二部分啦，是index.php的主要代码，暂时先不做分析。接下来我们看看这四个部分的关系，和他们在WordPress页面中的位置和作用，请看下面的这一图片。

<div id="attachment_180" style="width: 325px" class="wp-caption aligncenter">
  <a rel="attachment wp-att-180" href="http://blog.webfuns.net/archives/179.html/wpdesign02"><img class="size-full wp-image-180" title="wpdesign02" src="http://blog.webfuns.net/wp-content/uploads/2009/10/wpdesign02.gif" alt="wpdesign02" width="315" height="420" /></a>
  
  <p class="wp-caption-text">
    default主题四部分之间的关系
  </p>
</div>

<p style="text-align: center;">
  有任何问题可以在此留言或者im 我都可以！我会及时做出回复！也欢迎的大家对教程提出意见和建议，我们将做出及时的调整。
</p>
