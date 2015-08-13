---
date: Fri, 02 Mar 2012 14:44:23 +0000
title: 在Ubuntu上安装Node.js
author: tomheng
layout: post
permalink: /archives/1430.html
views: 372
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - Nodejs
---
<span style="color: #3366ff;">Node.js is a platform built on <a href="http://code.google.com/p/v8/"><span style="color: #3366ff;">Chrome&#8217;s JavaScript runtime</span></a> for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.</span>

上面是Node.js官网对自己的定义。要点如下：

  * 基于V8引擎
  * 目标是构建快速、高扩展的网络程序（不只是HTTP网站)
  * 事件驱动、非阻塞
  * 适于构建数据密集型实时程序

在Ubuntu上安装Node.js其实很简单：

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="kw2">wget</span> http:<span class="sy0">//</span>nodejs.org<span class="sy0">/</span>dist<span class="sy0">/</span>v0.6.11<span class="sy0">/</span>node-v0.6.11.tar.gz<br /> <span class="kw2">tar</span> <span class="re5">-xzvf</span> node-v0.6.11.tar.gz<br /> <span class="kw3">cd</span> node-v0.6.11<br /> <span class="kw2">sudo</span> <span class="kw2">apt-get install</span> <span class="kw2">g++</span> curl libssl-dev apache2-utils<br /> .<span class="sy0">/</span>configure<br /> <span class="kw2">make</span><br /> <span class="kw2">sudo</span> <span class="kw2">make</span> <span class="kw2">install</span>
        </div>
      </td>
    </tr>
  </table>
</div>

下面可以根据官网首页的代码，来用Node.js运行自己的Hello World程序了.
