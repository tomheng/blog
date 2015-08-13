---
date: Tue, 16 Jul 2013 05:18:07 +0000
title: SimplePie 将中文编码成 HTML Entities 的解决方法
author: tomheng
layout: post
permalink: /archives/1710.html
views: 247
categories:
  - LAMP
tags:
  - HTML Entities
  - SimplePie
---
闲来折腾一个玩具[五车书][1]的过程中，需要用到解析RSS的地方，于是找到了[强大的SimplePid][2]。在使用的过程中遇到了一些问题，比如前段时间遇到的[This XML document is invalid][3]的问题，其实在这问题前还有一个问题，就是今天说的这个中文被编码的问题。

经过一番折腾之后，终于搞明白了怎么回事并找到了BT的解决方案。

SimplePie 在library/Sanitize.php文件的sanitize方法中使用DOMDocument对数据进行了一次加工，正式这次加工把中文编码成了HTML Entities。

那么为什么会这样呢？php.net上在关于[DOMDocument::loadHTML][4]说明下面已经有高人讨论到了此问题。

<div class="codecolorer-container html4strict blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />17<br />18<br />19<br />20<br />21<br />22<br />23<br />24<br />25<br />
        </div>
      </td>
      
      <td>
        <div class="html4strict codecolorer">
          If you use loadHTML() to process utf HTML string (eg in Vietnamese), you may experience result in garbage text, while some files were OK. Even your HTML already have meta charset &nbsp;like<br /> <br /> &nbsp; <span class="sc2"><<a href="http://december.com/html/4/element/meta.html"><span class="kw2">meta</span></a> <span class="kw3">http-equiv</span><span class="sy0">=</span><span class="st0">"content-type"</span> <span class="kw3">content</span><span class="sy0">=</span><span class="st0">"text/html; charset=utf-8"</span>></span><br /> <br /> I have discovered that, to help loadHTML() process utf file correctly, the meta tag should come first, before any utf string appear. For example, this HTML file<br /> <br /> <span class="sc2"><<a href="http://december.com/html/4/element/html.html"><span class="kw2">html</span></a>></span><br /> &nbsp;<span class="sc2"><<a href="http://december.com/html/4/element/head.html"><span class="kw2">head</span></a>></span><br /> &nbsp; &nbsp; <span class="sc2"><<a href="http://december.com/html/4/element/meta.html"><span class="kw2">meta</span></a> <span class="kw3">http-equiv</span><span class="sy0">=</span><span class="st0">"content-type"</span> <span class="kw3">content</span><span class="sy0">=</span><span class="st0">"text/html; charset=utf-8"</span>></span><br /> &nbsp; &nbsp; <span class="sc2"><<a href="http://december.com/html/4/element/title.html"><span class="kw2">title</span></a>></span> Vietnamese - Tiếng Việt<span class="sc2"><<span class="sy0">/</span><a href="http://december.com/html/4/element/title.html"><span class="kw2">title</span></a>></span><br /> &nbsp; <span class="sc2"><<span class="sy0">/</span><a href="http://december.com/html/4/element/head.html"><span class="kw2">head</span></a>></span><br /> <span class="sc2"><<a href="http://december.com/html/4/element/body.html"><span class="kw2">body</span></a>><<span class="sy0">/</span><a href="http://december.com/html/4/element/body.html"><span class="kw2">body</span></a>></span><br /> <span class="sc2"><<span class="sy0">/</span><a href="http://december.com/html/4/element/html.html"><span class="kw2">html</span></a>></span><br /> <br /> will be OK with loadHTML() when <span class="sc2"><<a href="http://december.com/html/4/element/meta.html"><span class="kw2">meta</span></a>></span> tag appear <span class="sc2"><<a href="http://december.com/html/4/element/title.html"><span class="kw2">title</span></a>></span> tag.<br /> <br /> But the file below will not regcornize by loadHTML() because <span class="sc2"><<a href="http://december.com/html/4/element/title.html"><span class="kw2">title</span></a>></span> tag contains utf string appear before <span class="sc2"><<a href="http://december.com/html/4/element/meta.html"><span class="kw2">meta</span></a>></span> tag.<br /> <br /> <span class="sc2"><<a href="http://december.com/html/4/element/html.html"><span class="kw2">html</span></a>></span><br /> &nbsp;<span class="sc2"><<a href="http://december.com/html/4/element/head.html"><span class="kw2">head</span></a>></span><br /> &nbsp; &nbsp; <span class="sc2"><<a href="http://december.com/html/4/element/title.html"><span class="kw2">title</span></a>></span> Vietnamese - Tiếng Việt<span class="sc2"><<span class="sy0">/</span><a href="http://december.com/html/4/element/title.html"><span class="kw2">title</span></a>></span><br /> &nbsp; &nbsp; <span class="sc2"><<a href="http://december.com/html/4/element/meta.html"><span class="kw2">meta</span></a> <span class="kw3">http-equiv</span><span class="sy0">=</span><span class="st0">"content-type"</span> <span class="kw3">content</span><span class="sy0">=</span><span class="st0">"text/html; charset=utf-8"</span>></span><br /> &nbsp; <span class="sc2"><<span class="sy0">/</span><a href="http://december.com/html/4/element/head.html"><span class="kw2">head</span></a>></span><br /> <span class="sc2"><<a href="http://december.com/html/4/element/body.html"><span class="kw2">body</span></a>><<span class="sy0">/</span><a href="http://december.com/html/4/element/body.html"><span class="kw2">body</span></a>></span><br /> <span class="sc2"><<span class="sy0">/</span><a href="http://december.com/html/4/element/html.html"><span class="kw2">html</span></a>></span>
        </div>
      </td>
    </tr>
  </table>
</div>

SimplePie其实已经通过的Santinize的preprocess方法加上了合理charset，按照上面的说明中文就不应该被编码了。看起来凶手另有其人啊，仔细看代码后发现，问题出现在SimplePie在saveHTML之前为了原样取回数据进行了一个处理，代码如下。

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="re0">$real_body</span> <span class="sy0">=</span> <span class="re0">$document</span><span class="sy0">-></span><span class="me1">getElementsByTagName</span><span class="br0">&#40;</span><span class="st_h">'body'</span><span class="br0">&#41;</span><span class="sy0">-></span><span class="me1">item</span><span class="br0">&#40;</span><span class="nu0"></span><span class="br0">&#41;</span><span class="sy0">-></span><span class="me1">childNodes</span><span class="sy0">-></span><span class="me1">item</span><span class="br0">&#40;</span><span class="nu0"></span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="re0">$document</span><span class="sy0">-></span><span class="me1">replaceChild</span><span class="br0">&#40;</span><span class="re0">$real_body</span><span class="sy0">,</span> <span class="re0">$document</span><span class="sy0">-></span><span class="me1">firstChild</span><span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

这样处理的结果就像重新load一次HTML一样，但是因为这次每页charset设置，所以中文在这里被编码成了HTML Entities。

知道了原因就好办了，我是这样搞定她滴～。  
如下修改library/Sanitize.php文件即可。

<div class="codecolorer-container php blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />15<br />16<br />
        </div>
      </td>
      
      <td>
        <div class="php codecolorer">
          <span class="co1">//修复开始</span><br /> <span class="re0">$unique_tag</span> <span class="sy0">=</span> <span class="st_h">'#'</span><span class="sy0">.</span><a href="http://www.php.net/uniqid"><span class="kw3">uniqid</span></a><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">.</span><span class="st_h">'#'</span><span class="sy0">;</span><br /> <span class="re0">$data</span> <span class="sy0">=</span> <span class="re0">$unique_tag</span><span class="sy0">.</span><span class="re0">$data</span><span class="sy0">.</span><span class="re0">$unique_tag</span><span class="sy0">;</span><br /> <span class="co1">//修复结束</span><br /> <span class="re0">$data</span> <span class="sy0">=</span> <span class="re0">$this</span><span class="sy0">-></span><span class="me1">preprocess</span><span class="br0">&#40;</span><span class="re0">$data</span><span class="sy0">,</span> <span class="re0">$type</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="sy0">...</span><br /> <span class="sy0">...</span><br /> <span class="co1">// Move everything from the body to the root</span><br /> <span class="co1">//注释掉下面紧邻的两行</span><br /> <span class="co1">//$real_body = $document->getElementsByTagName('body')->item(0)->childNodes->item(0);</span><br /> <span class="co1">//$document->replaceChild($real_body, $document->firstChild);</span><br /> <br /> <span class="co1">// Finally, convert to a HTML string</span><br /> <span class="re0">$data</span> <span class="sy0">=</span> <a href="http://www.php.net/trim"><span class="kw3">trim</span></a><span class="br0">&#40;</span><span class="re0">$document</span><span class="sy0">-></span><span class="me1">saveHTML</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span><br /> <span class="co1">//添加此行</span><br /> <a href="http://www.php.net/list"><span class="kw3">list</span></a><span class="br0">&#40;</span><span class="re0">$_</span><span class="sy0">,</span> <span class="re0">$data</span><span class="sy0">,</span> <span class="re0">$_</span><span class="br0">&#41;</span> <span class="sy0">=</span> <a href="http://www.php.net/explode"><span class="kw3">explode</span></a><span class="br0">&#40;</span><span class="re0">$unique_tag</span><span class="sy0">,</span> <span class="re0">$data</span><span class="br0">&#41;</span><span class="sy0">;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

 [1]: http://126.am/fbooks "将阅读进行到底"
 [2]: http://simplepie.org/ "SimplePie is a very fast and easy-to-use feed parser, written in PHP, that puts the 'simple' back into 'really simple syndication'."
 [3]: http://blog.webfuns.net/archives/1681.html
 [4]: http://www.php.net/manual/en/domdocument.loadhtml.php
