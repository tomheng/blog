---
date: Sun, 27 Oct 2013 13:27:22 +0000
title: Ubuntu 升级到13.10后Chromium 的 extension 都不工作了
author: tomheng
layout: post
permalink: /archives/1750.html
views: 108
categories:
  - Google
tags:
  - Chrome
  - Ubuntu
---
Canonical 前段时间刚发布了[ Ubuntu 13.10 (Saucy)][1] , 作为Ubuntu的死忠赶紧升级到最新版本，但是悲剧接着就发生了，[Chromium ][2] 的所有扩展(extension)都不能用了，找了大半天也没有找到很好的解决方案。

最后只好换了[Google Chrome][3]，搞定之后发现还是chrome要好用些。

通过下面的命令就可以安装：

<div class="codecolorer-container bash blackboard" style="overflow:auto;white-space:nowrap;">
  <table cellspacing="0" cellpadding="0">
    <tr>
      <td class="line-numbers">
        <div>
          1<br />2<br />3<br />4<br />5<br />6<br />7<br />8<br />
        </div>
      </td>
      
      <td>
        <div class="bash codecolorer">
          <span class="sy0">//</span> add key<br /> <span class="kw2">wget</span> <span class="re5">-q</span> <span class="re5">-O</span> - https:<span class="sy0">//</span>dl-ssl.google.com<span class="sy0">/</span>linux<span class="sy0">/</span>linux_signing_key.pub <span class="sy0">|</span> <span class="kw2">sudo</span> <span class="kw2">apt-key add</span> -<br /> <span class="sy0">//</span> add it to the repository<br /> <span class="kw2">sudo</span> <span class="kw2">sh</span> <span class="re5">-c</span> <span class="st_h">'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'</span><br /> <span class="sy0">//</span> update<br /> <span class="kw2">sudo</span> <span class="kw2">apt-get update</span><br /> <span class="sy0">//</span> <span class="kw2">install</span> google chrome<br /> <span class="kw2">sudo</span> <span class="kw2">apt-get install</span> google-chrome-stable
        </div>
      </td>
    </tr>
  </table>
</div>

也可以直接去chrome官网下载deb包，直接安装就好了。

PS：[Chrome 和 Chromium 的区别][4]

 [1]: http://releases.ubuntu.com/saucy/
 [2]: http://www.chromium.org/ "The Chromium Projects"
 [3]: https://www.google.com/intl/en/chrome/browser/
 [4]: https://code.google.com/p/chromium/wiki/ChromiumBrowserVsGoogleChrome
