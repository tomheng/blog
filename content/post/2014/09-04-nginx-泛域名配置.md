---
date: Thu, 04 Sep 2014 06:20:35 +0000
title: Nginx 泛域名配置
author: tomheng
layout: post
permalink: /archives/1845.html
views: 153
categories:
  - LAMP
tags:
  - Nginx
---
泛域名的应用比较广泛，比如实现二级域名的无限支持、<span style="color: #333333;">免费的URL转发等。在开发中，如果团队中多个成员须测试同一套代码，那么就会出现彼此覆盖冲突的问题。这个时候就可以用泛域名将不同的域名指向到不同的目录，这样就不会彼此干扰了。</span>

相关配置如下

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          server<br /> <span class="br0">&#123;</span><br /> &nbsp; &nbsp;<span class="kw1">set</span> <span class="re1">$domain</span> <span class="sy0">/</span>data1<span class="sy0">/</span>apache<span class="sy0">/</span>share<span class="sy0">/</span>htdocs<span class="sy0">/</span><span class="kw3">test</span>;<br /> &nbsp; &nbsp;<span class="kw1">if</span> <span class="br0">&#40;</span><span class="re1">$http_host</span> ~<span class="sy0">*</span> <span class="st0">"^(.*)\.test\.com$"</span><span class="br0">&#41;</span><br /> &nbsp; &nbsp;<span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">set</span> <span class="re1">$domain</span> <span class="re4">$1</span>;<br /> &nbsp; &nbsp;<span class="br0">&#125;</span><br /> &nbsp; &nbsp;root <span class="sy0">/</span>usr<span class="sy0">/</span>home<span class="sy0">/</span><span class="re1">$domain</span><span class="sy0">/</span><span class="kw3">test</span>;<br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

这样A开发可以用a.test.com , B开可以用b.test.com 。

&#8211;done&#8211;
