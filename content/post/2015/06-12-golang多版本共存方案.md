---
date: Fri, 12 Jun 2015 08:35:04 +0000
title: Golang多版本共存方案
author: tomheng
layout: post
permalink: /archives/1936.html
views: 85
categories:
  - LAMP
tags:
  - Golang
  - Russ cox
---
如果你是一个狂热的Golang爱好者，也许你会同时使用两个或两个以上的Golang版本。那么这时候怎么实现呢？

这里提供一个Russ cox的方案。这个方案是在Google Group里看到的，但是我现在已经找不到具体是哪个链接了。

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="co0">#file:/usr/bin/go1.4</span><br /> <span class="co0">#!/bin/bash</span><br /> <span class="kw3">export</span> <span class="re2">GOROOT</span>=<span class="re1">$HOME</span><span class="sy0">/</span>go1.4<br /> <span class="kw3">export</span> <span class="re2">PATH</span>=<span class="re1">$HOME</span><span class="sy0">/</span>go1.4<span class="sy0">/</span>bin:<span class="re1">$PATH</span><br /> <span class="kw3">exec</span> <span class="re1">$GOROOT</span><span class="sy0">/</span>bin<span class="sy0">/</span>go <span class="st0">"$@"</span>
        </div>
      </td>
    </tr>
  </table>
</div>

历史稳定版本可以这样处理，然后把开发版本作为默认的。

all done.

另：[Russ cox ][1]已经成为我的偶像啦

 [1]: https://twitter.com/_rsc
