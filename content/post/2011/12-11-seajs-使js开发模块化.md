---
date: Sun, 11 Dec 2011 15:33:31 +0000
title: seajs 使js开发模块化
author: tomheng
layout: post
permalink: /archives/1246.html
views: 499
robotsmeta:
  - index,follow
categories:
  - LAMP
tags:
  - JavaScript
  - seajs
---
[seajs][1]一个js类库，主要实现对js的模块化开发。

对于一个大项目或者是一个需要团队协作的项目，模块化开发

可以使得对代码的管理更规范和高效。seajs就是为了满足这种需求来的。

不过我个人可能会更需要js可以按需加载，因为有些类库不是所有的页面都

需要的，我的预期是js类库只有在需要的时候才去加载。

seajs可以通过require.async来实现按需加载。

最近在关注HTML5、CSS3的相关内容，这两个东西确实会给我们带来很不一样的体验。

还有js也很活跃，后端出了[nodejs][2]，前段有各种基础类库，还有MVC框架。

还有在js上发展起来的新语言[ coffee script][3]和[dart][4],令人目不暇接。

确实很多东西([以论坛为主的网站将如何转型？][5])需要从底层变革一下了，因为他们周围的东西已经变了。

矛盾促使了这样的变革和发展。

更多内容：seajs.com

 [1]: seajs.com
 [2]: http://nodejs.org/
 [3]: http://jashkenas.github.com/coffee-script/
 [4]: http://www.dartlang.org/
 [5]: http://www.zhihu.com/question/19938648
