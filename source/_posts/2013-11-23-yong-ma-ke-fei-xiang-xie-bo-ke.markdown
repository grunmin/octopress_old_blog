---
layout: post
title: "用马克飞象写博客"
date: 2013-11-23 15:46
comments: true
categories: octopress 
tags: octopress 笔记 随记
---

用octopress搭建博客之后，大爱markdown的简约，因此我对云笔记的要求又多了一项：支持markdown格式编辑和保存。  

我之前尝试用过一些云笔记产品，比如印象笔记，为之笔记，有道云笔记等。印象笔记的网页版很好，但是linux下没有官方的客户端（这意味着不能离线查看笔记）。为之笔记我一直在用着，因为它有linux客户端，但它的是网页版很糟糕，而且最近出现了网页版加载不了笔记的情况，因此有换个笔记的想法。有道云就不提了，目前没发现什么优势。  

<!-- more -->

在写octopress博客的时候，我先在为之笔记上写好txt格式的正文，将其复制到stackedit上进行markdown处理，再将转化后的内容写进octopress的文件中。这样，在为之笔记上的博客内容就和octopress上的不同了，而且，笔记上无格式的正文也十分难看。 

今天在chrome商店上看到了一个应用，据称是专为印象笔记打造的markdown编辑器，叫马克飞象。这是图标  
![enter image description here][1]

绑定笔记帐号后在上面编辑可以实时查看效果，并将处理后的笔记保存到印象笔记中。效果如图：
![enter image description here][2]


用马克飞象编辑的文章保存到笔记中是markdown解释过的语言（笔记不会显示源代码），这样保存下来的笔记比无格式或者markdown源代码格式的好看多了。  相对于stackedit与谷歌云端硬盘的优势也是十分明显的，它可以直接在印象笔记中查看。

马克飞象支持将所编辑的笔记分类和加标签，使用`@(categories)[tags|tags]`即可，同一文章中只能出现一次，且必须与其他内容隔一些距离。

用马克飞象编辑的笔记在右上角会显示一个链接，点击链接后会用启用马克飞象编辑文章，因此不用担心后续更新的问题。


![enter image description here][3]

我是用马克飞象编辑的这篇博客，在编辑过程中也发现了一些问题。问题有三：

  - 插入的图片不能显示  
  编辑时插入图片不能显示，保存到印象笔记后也是如此。
  - 编辑器的光标显示位置不准确  
  这是个更大的问题，光标显示所在的位置不是其真实位置，在添删个别文字时特别麻烦。
  - 跟其他编辑有部分不兼容(比如stackedit)

总体来说马克飞象还是十分实用的，特别是对程序员，用马克飞象记录代码显得如虎添翼。


  [1]: https://lh5.googleusercontent.com/-hOEvL1TOB-o/UpBepDGUAhI/AAAAAAAAAGY/WHZ5qubwNeE/s0/2013-11-23-151125_118x133_scrot.png "2013-11-23-151125_118x133_scrot.png"
  [2]: https://lh6.googleusercontent.com/-N6XlKlK81F8/UpBe4QtcpWI/AAAAAAAAAGk/GtCzYl-__PY/s0/2013-11-23-153122_422x354_scrot.png "2013-11-23-153122_422x354_scrot.png"
  [3]: https://lh6.googleusercontent.com/-aipDbqSYjS0/UpBe_0jaxzI/AAAAAAAAAGw/giigXjHPP2A/s0/2013-11-23-153132_937x329_scrot.png "2013-11-23-153132_937x329_scrot.png"
