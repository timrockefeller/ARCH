---
title: 遇见hexo
date: 2018-04-18 08:24:37
tags: coding
---

---

的路总是不平坦。

使用`sequence`代码块的时候，使用了[hexo-filter-sequence](https://github.com/bubkoo/hexo-filter-sequence)模块。

没想到并没有成功

在issue里面看到有人这样做就成功了：

 - 在renderer.js文件里加上
`data.content += '<script src="' + config.raphael + '"></script>';`
 - 在index.js文件里加上
`raphael: 'https://cdnjs.cloudflare.com/ajax/libs/raphael/2.2.7/raphael.min.js',`
 - 运行hexo clean hexo s -g，好像可以暂时解决问题。

 还真的解决了问题。

 但谷歌的ajax却没有办法，~~下辈子~~哪天把theme重写一遍。