---
date: Tue, 24 Dec 2013 14:00:53 +0000
title: 小网站也可以用上自己的HTTPS
author: tomheng
layout: post
permalink: /archives/1766.html
views: 309
categories:
  - LAMP
tags:
  - HTTPS
  - Nginx
---
自己周末吓折腾一番，总结下分享出来，希望对大家有用。

其实就两步就可以完成，超简单的哦～

**1）申请证书**

这个一般正规网站都用收费的证书，小网站或者像我这样的博客就没有必要用收费证书了。我选择了[StartSSL][1],申请的时候按照网站的说明一步步来有七八分钟就能够拿到证书，如果实在搞不定可以看看这个[说明][2]。

请注意，从StartSSL申请的免费证书只有一年的有效期。

**2）在服务器上安装证书**

所谓的证书就是一个字符串，这个上传到服务器指定目录，然后配置下服务器就可以了。如果是nginx可以参看<http://www.startssl.com/?app=42>。

下面是我的服务器配置，顺便支持了SPDY。

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
          server <span class="br0">&#123;</span><br /> &nbsp; &nbsp; listen <span class="nu0">443</span> ssl spdy;<br /> &nbsp; &nbsp; server_name blog.webfuns.net;<br /> &nbsp; &nbsp; root &nbsp; <span class="sy0">/</span>var<span class="sy0">/</span>www<span class="sy0">/</span>blog<span class="sy0">/</span>;<br /> &nbsp; &nbsp; &nbsp; &nbsp; ssl on;<br /> &nbsp; &nbsp; ssl_certificate <span class="sy0">/</span>etc<span class="sy0">/</span>nginx<span class="sy0">/</span>certs<span class="sy0">/</span>ssl-unified.crt;<br /> &nbsp; &nbsp; ssl_certificate_key <span class="sy0">/</span>etc<span class="sy0">/</span>nginx<span class="sy0">/</span>certs<span class="sy0">/</span>server.key;<br /> &nbsp; &nbsp; index &nbsp;index.html index.htm index.php;<br /> <span class="br0">&#125;</span>
        </div>
      </td>
    </tr>
  </table>
</div>

接下来把服务器重启下就可以啦，打开浏览器试试<https://blog.webfuns.net>。

顺祝大家圣诞快乐～

 [1]: http://www.startssl.com/
 [2]: http://www.deepvps.com/apply-startssl-ssl-certificate.html
