---
date: Thu, 11 Jun 2009 14:57:35 +0000
title: php中的全局变量（$_POST,$_GET,······）关系整理
author: tomheng
layout: post
permalink: /archives/58.html
robotsmeta:
  - index,follow
views: 938
categories:
  - LAMP
tags:
  - LAMP
---
<p style="text-align: center;">
  <div id="attachment_599" style="width: 655px" class="wp-caption aligncenter">
    <a href="http://blog.webfuns.net/wp-content/uploads/2009/06/global.jpg"><img class="size-full wp-image-599 " title="PHP中的全局变量" src="http://blog.webfuns.net/wp-content/uploads/2009/06/global.jpg" alt="PHP中的全局变量" width="645" height="240" /></a>
    
    <p class="wp-caption-text">
      PHP中的全局变量
    </p>
  </div>
  
  <div class="Section0">
    <h2>
      <span style="font-family: mceinline;"><span style="font-family: mceinline;">总结：</span></span>
    </h2>
    
    <p class="p0" style="padding-left: 30px;">
      1<span style="font-family: 宋体;">）</span><span style="font-family: 'Times New Roman';">php</span><span style="font-family: 宋体;">中的全局变量主要分为系统自动全局变量和用户自定义的全局变量。系统自动全局变量包括：</span><span style="font-family: 'Times New Roman';">$_POST</span><span style="font-family: 宋体;">、 </span><span style="font-family: 'Times New Roman';">$_GET</span><span style="font-family: 宋体;">、</span><span style="font-family: 'Times New Roman';">$_COOKIE</span><span style="font-family: 宋体;">、</span><span style="font-family: 'Times New Roman';">$SESSION</span><span style="font-family: 宋体;">等（图示中标为同一颜色的），</span>
    </p>
    
    <p class="p0" style="padding-left: 30px;">
      用户自定义的全局变量例如：<span style="font-family: 'Times New Roman';">global $a;</span><span style="font-family: 宋体;">此处</span><span style="font-family: 'Times New Roman';">$a</span><span style="font-family: 宋体;">即为全局变量。</span>
    </p>
    
    <p class="p0" style="padding-left: 30px;">
      2）关于全局变量的访问：
    </p>
    
    <p class="p0" style="padding-left: 60px;">
      #<span style="font-family: 宋体;">）所有的全局变量都可以通过</span><span style="font-family: 'Times New Roman';">$GLOBALS[$name]</span><span style="font-family: 宋体;">访问。</span>
    </p>
    
    <p class="p0" style="padding-left: 60px;">
      #<span style="font-family: 宋体;">）</span><span style="font-family: 'Times New Roman';">$HTTP_*_VARS</span><span style="font-family: 宋体;">是旧式的访问方法，这要保证</span><span style="font-family: 'Times New Roman';">register_long_array</span><span style="font-family: 宋体;">是打开的。但是这种方式无法访问</span><span style="font-family: 'Times New Roman';">$_REQUEST</span><span style="font-family: 宋体;">变量和用户定义的变量。图示中有箭头指向</span><span style="font-family: 'Times New Roman';">$HTTP_*_VARS</span><span style="font-family: 宋体;">就表示能够访问。</span>
    </p>
    
    <p class="p0" style="padding-left: 60px;">
      #<span style="font-family: 宋体;">）对于不同类的变量可以用不同的方法访问，例如</span><span style="font-family: 'Times New Roman';">cookie</span><span style="font-family: 宋体;">类变量可以用</span><span style="font-family: 'Times New Roman';">$_COOKIE[$name]</span><span style="font-family: 宋体;">访问。</span>
    </p>
    
    <p class="p0" style="padding-left: 60px;">
      #<span style="font-family: 宋体;">）</span><span style="font-family: 'Times New Roman';">$_ENV $_SERVER $_GET</span><span style="font-family: 宋体;">这些变量有些特殊。比如</span><span style="font-family: 'Times New Roman';">$_ENV[$key],</span><span style="font-family: 宋体;">也可以通过</span><span style="font-family: 'Times New Roman';">$GLOBALS[$key]</span><span style="font-family: 宋体;">访问。</span>
    </p>
    
    <p class="p0" style="padding-left: 60px;">
      #)$_REQUEST<span style="font-family: 宋体;">变量包含</span><span style="font-family: 'Times New Roman';">$_GET $_POST $_COOKIE</span><span style="font-family: 宋体;">的变量。就是</span><span style="font-family: 'Times New Roman';">$_GET[$key]</span><span style="font-family: 宋体;">，可以通过</span><span style="font-family: 'Times New Roman';">$_REQUEST[$key]</span><span style="font-family: 宋体;">访问。</span>
    </p>
    
    <h2>
      实例：
    </h2>
    
    <p class="p0" style="padding-left: 30px;">
      比如我们要访问通过<span style="font-family: 'Times New Roman';">url</span><span style="font-family: 宋体;">传过来的变量：</span><a href="http://blog.webfuns.net?id"><span class="15">http://blog.webfuns.net?id</span></a>=tomheng
    </p>
    
    <p class="p0" style="padding-left: 30px;">
      以下几种方式都可以访问到这些变量：
    </p>
    
    <p class="p0" style="padding-left: 30px;">
      $_GET[&#8216;id&#8217;]=$_REQUEST[&#8216;id&#8217;]=$GLOBLAS[&#8216;_GET&#8217;][&#8221;id&#8217;]
    </p>
    
    <p class="p0" style="padding-left: 30px;">
      如果<span style="font-family: 'Times New Roman';">register_long_array</span><span style="font-family: 宋体;">是打开的</span><span style="font-family: 'Times New Roman';">:$_HTTP_GET_VARS[&#8216;id&#8217;]</span><span style="font-family: 宋体;">也可以访问（不安全，一般不适用这种方式）</span>
    </p>
    
    <p class="p0" style="padding-left: 30px;">
      如果<span style="font-family: 'Times New Roman';">register_global </span><span style="font-family: 宋体;">也打开的换，</span><span style="font-family: 'Times New Roman';">$id</span><span style="font-family: 宋体;">也可以访问（不安全，一般不适用这种方式）</span>
    </p>
    
    <h2>
      唠叨：
    </h2>
    
    <p class="p0" style="padding-left: 30px;">
      这样一整理可能会更容易理解一下，其实在实践中全局变量没那么复杂，但是这样整理清楚他们的关系之后在一些细节问题上还是有帮助的。
    </p>
  </div>
