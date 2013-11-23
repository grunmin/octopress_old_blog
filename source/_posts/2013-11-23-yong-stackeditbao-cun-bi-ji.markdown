---
layout: post
title: "用stackedit保存笔记"
date: 2013-11-23 16:49
comments: true
categories: 随记
tags: octopress 笔记 随记
---
在[上一篇文章](http://grunmin.github.io/blog/2013/11/23/yong-ma-ke-fei-xiang-xie-bo-ke/)里写到用马克飞象写博客，不知对用印象笔记的读者有没有帮助。我对工具有近乎狂热的追求，花了很多时间来尝试不同工具，试图找出其中最高效率的，最简约的，最人性化的。比如云盘，云相册，linux发行版，以及各种编辑器等。在写上一篇博客的时候其实也用到了其他的工具，我在此将其写下来。  

<!-- more -->

马克飞象编辑器显然对markdown的支持没有专业工具那么好（还有上文提到的硬伤)，在发表一片博文之前，我都要将其拷贝到stackedit上查看格式是否有误，在stackedit上修改之后又拷贝到笔记里。过程繁琐，于是我寻思，是否有更加简单的方式来完成同样的工作。  

其实stackedit提供了我需要的功能。它支持编辑器与谷歌云端硬盘和dropbox之间文件的同步。也就是说，在stackedit上编辑的文件可以保存到谷歌和dropbox的服务器中，这其实就是用markdown来写笔记。  

点击stackedit页面左上角的图标![enter image description here][1]，在SYNCHRONIZE下有谷歌云端硬盘和dropbox的图标，点击`Export to Google Drive`就可以将文章保存到云端硬盘中，dropbox也是如此。  

如此，我发表博文的过程相对简单了。直接在stackedit上编辑（支持离线编辑）好，保存到云端，然后把代码交给octopress处理即可。  

用stackedit有另一个好处是可以直接插入谷歌相册里的图片，或者直接上传本地图片到相册上直接用。  

到目前为止，我发现的唯一缺点是谷歌文档不能预览或编辑stackedit写好的笔记，需要通过stackedit来打开和编辑。


  [1]: https://lh3.googleusercontent.com/DeduTfgiszvG6lZ-SjC0XvCWAJELtr6026s9wQWR9g=s0
