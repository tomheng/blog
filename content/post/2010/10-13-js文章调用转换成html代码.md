---
date: Wed, 13 Oct 2010 07:00:37 +0000
title: JS文章调用转换成HTML代码
author: tomheng
layout: post
permalink: /archives/842.html
robotsmeta:
  - index,follow
views: 807
categories:
  - LAMP
---
我们有时候需要进行站外的内容调用，而这时候一般使用JS调用方式，但是这种方式不够友好，搜索引擎读不出JS的输出，但是不同的网站系统又有很多差别所以很难写出兼容所有系统的代码，不够前段时间我想到了一个办法，很笨但是确实是可以使用的。

**函数原型**：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="kw2">function</span> js2html<span class="br0">&#40;</span><span class="re0">$js_url</span><span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; &nbsp; <span class="re0">$content</span><span class="sy0">=</span><a href="http://www.php.net/file_get_contents"><span class="kw3">file_get_contents</span></a><span class="br0">&#40;</span><span class="re0">$js_url</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="co1">//$replace=array("document.write('","document.write(\"",")","");</span><br /> &nbsp; &nbsp; <span class="re0">$content</span><span class="sy0">=</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"');"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"document.write('"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><span class="re0">$content</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="re0">$content</span><span class="sy0">=</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"<span class="es1">\"</span>);"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"document.write(<span class="es1">\"</span>"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><span class="re0">$content</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="kw1">echo</span> <span class="re0">$content</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

**使用：**  
比如原先的js调用是这样的：

<div class="codecolorer-container javascript blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />
        </div>
      </td>
      
      <td>
        <div class="javascript codecolorer">
          <span class="sy0"><</span>script <br /> src<span class="sy0">=</span><span class="st0">"http://www.wuxianle.com/bbs/new.php?action=article&digest=0&postdate=0&author=1&fname=0&hits=0&replies=0&pre=1&num=5&length=35&order=2"</span> type<span class="sy0">=</span><span class="st0">"text/javascript"</span><span class="sy0">></span><br /> <span class="sy0"></</span>script<span class="sy0">></span>
        </div>
      </td>
    </tr>
  </table>
</div>

现在可以替换成：

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="kw2">function</span> js2html<span class="br0">&#40;</span><span class="re0">$js_url</span><span class="br0">&#41;</span><span class="br0">&#123;</span><br /> &nbsp; &nbsp; <span class="re0">$content</span><span class="sy0">=</span><a href="http://www.php.net/file_get_contents"><span class="kw3">file_get_contents</span></a><span class="br0">&#40;</span><span class="re0">$js_url</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="co1">//$replace=array("document.write('","document.write(\"",")","");</span><br /> &nbsp; &nbsp; <span class="re0">$content</span><span class="sy0">=</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"');"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"document.write('"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><span class="re0">$content</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="re0">$content</span><span class="sy0">=</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"<span class="es1">\"</span>);"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><a href="http://www.php.net/str_replace"><span class="kw3">str_replace</span></a><span class="br0">&#40;</span><span class="st0">"document.write(<span class="es1">\"</span>"</span><span class="sy0">,</span><span class="st0">""</span><span class="sy0">,</span><span class="re0">$content</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> &nbsp; &nbsp; <span class="kw1">echo</span> <span class="re0">$content</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span><br /> js2html<span class="br0">&#40;</span><span class="st0">"http://www.wuxianle.com/bbs/new.php?action=article&digest=0&postdate=0&author=1&fname=0&hits=0&replies=0&pre=1&num=5&length=35&order=2"</span><span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

基本思路就是这样的，我想可以通过正则实现更好的功能，不够现在没有时间去做，等过些时候吧。还有可以结合[Ecall外部调用插件][1]使用，这样就可以使用Ecall生产html的外部调用了。看来还是挺有用的！

 [1]: http://blog.webfuns.net/archives/358.html
