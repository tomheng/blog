---
date: Wed, 16 Nov 2011 05:52:07 +0000
title: ubuntu 下连接cisco vpn
author: tomheng
layout: post
permalink: /archives/1197.html
views: 911
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - cisco
  - Ubuntu
  - vpn
---
命令方式连接

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />9<br />10<br />11<br />12<br />13<br />14<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="co0">#1）安装vpnc</span><br /> <span class="kw2">sudo</span> <span class="kw2">apt-get install</span> vpnc<br /> <span class="co0">#2) 添加配置文件(/etc/vpnc目录下有example.conf示例配置文件)</span><br /> <span class="co0">#配置文件示例：</span><br /> <span class="co0">#IPSec gateway 192.168.1.0</span><br /> <span class="co0">#IPSec ID groupname</span><br /> <span class="co0">#IPSec secret group_password</span><br /> <span class="co0">#Xauth username &nbsp;username</span><br /> <span class="co0">#Xauth password &nbsp;userpassword</span><br /> <span class="kw2">sudo</span> <span class="kw2">vim</span> <span class="sy0">/</span>etc<span class="sy0">/</span>vpnc<span class="sy0">/</span>myvpn.conf<br /> <span class="co0">#3）启动</span><br /> <span class="kw2">sudo</span> vpnc <span class="re5">--enable-1des</span> vpnc<span class="sy0">/</span>hvpn<br /> <span class="co0">#4）断开连接</span><br /> <span class="kw2">sudo</span> vpnc-disconnect
        </div>
      </td>
    </tr>
  </table>
</div>

上面是通过配置文件连接VPN,当然也可以通过sudo vpnc &#8211;enable-1des直接启动，然后根据提示输入相关信息。

参考：http://www.linuxplanet.com/linuxplanet/tutorials/6773/1/
