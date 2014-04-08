---
layout: post
title: "用马克飞象写博客"
date: 2013-11-23 15:46
comments: true
categories: 技术
tags: 工具
---

用octopress搭建博客之后，大爱markdown的简约，因此我对云笔记的要求又多了一项：支持markdown格式编辑和保存。  

应该说每一种云笔记都有自己的特点。为知笔记在知识管理方面有独到之处，它的网页剪辑功能强大，支持无限制分级，且全平台支持，可作为PKM工具。印象笔记界面清爽，多种应用接入，逐步打造成完整的生态系统。而现在，为知笔记和印象笔记都支持markdown了，诸君可以试用看哪一种好。本文只谈技术，不贸然推荐。

在写octopress博客的时候，我先在为知笔记上写好纯文本格式的正文，将其复制到stackedit上用markdown语法进行处理，确认无误后写进octopress的文件中。这样的问题是，更新博文后笔记和博客的内容就不一样了。再说，笔记上纯文本格式的正文也十分难看。 （这是写在为知笔记还不支持markdown时候的情况，事实上自从它支持后就不用那么麻烦了。）

今天在chrome商店上看到了一个应用，据称是专为印象笔记打造的markdown编辑器，叫马克飞象。这是图标  
![enter image description here][1]

绑定笔记帐号后在上面编辑md格式文件可以实时查看效果，并将处理后的笔记保存到印象笔记中。这是编辑界面:

![enter image description here][3]


用马克飞象编辑的文章保存到笔记中是经markdown渲染的（笔记不会显示源代码），这样保存下来的笔记比纯文本格式或者markdown源代码格式的好看多了。 

相对于stackedit与谷歌云端硬盘的优势也是十分明显的，它可以直接在印象笔记中查看。

马克飞象支持将所编辑的笔记分类和加标签，使用`@(categories)[tags|tags]`即可，同一文章中只能出现一次，且必须与其他内容隔一些距离。

如果要修改笔记内容怎么办？不用担心，用马克飞象编辑的笔记在右上角会显示一个链接（如下图），点击链接后会用启用马克飞象编辑文章，因此不用担心后续更新的问题。

![enter image description here][2]


我用马克飞象写的这篇博文，感觉非常不错，不过有两点需要注意：

  - 图片显示  
  直接写`![][]`这样插入图片的语法能够生效，但是在编辑器上无法看到预览。要看到预览，请点击右上角`功能`里插入图片的选项。
  - 兼容问题
  由于md语言有多种扩展，将马克飞象已显示无问题的源码直接拷贝到octopress里可能并不能达到预想的效果。

总体来说马克飞象还是十分实用的，特别是对程序员，用马克飞象记录代码如虎添翼。


  [1]: https://lh5.googleusercontent.com/-hOEvL1TOB-o/UpBepDGUAhI/AAAAAAAAAGY/WHZ5qubwNeE/s0/2013-11-23-151125_118x133_scrot.png "2013-11-23-151125_118x133_scrot.png"
  [2]: https://lh6.googleusercontent.com/-N6XlKlK81F8/UpBe4QtcpWI/AAAAAAAAAGk/GtCzYl-__PY/s0/2013-11-23-153122_422x354_scrot.png "2013-11-23-153122_422x354_scrot.png"
  [3]: https://lh6.googleusercontent.com/-aipDbqSYjS0/UpBe_0jaxzI/AAAAAAAAAGw/giigXjHPP2A/s0/2013-11-23-153132_937x329_scrot.png "2013-11-23-153132_937x329_scrot.png"
