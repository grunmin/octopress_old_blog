---
layout: post
title: "我的gvim安装和配置"
date: 2013-11-17 17:14
comments: true
tags: vim
categories: vim
---
我将自己正在学习的小东西放到了github上，想要充分发挥云端服务器的优势，希望在其他windows电脑也能coding，因此草草配置了一下gvim。
<!-- more -->

我在linux下已经对vim有了很好的配置，在编写php文件时十分顺利方便。不想在windows下也重复这一过程（我配置linux下的vim花了几天时间才宣告满意收工），因此把linux下的所有文件都拿了过来，用极其简单粗暴的方式放进gvim的安装文件中。到目前为止，除了一些小问题，工作得还顺利，因此将这一过程记录下来。



-  [安装Gvim](http://www.vim.org/download.php#pc)

-  安装[中文帮助](http://vimcdoc.sourceforge.net/)

-  下载linux下的[vim](http://pan.baidu.com/s/1tcJSm)文件  
- 将.vim的plugin和doc夹下的全部内容添加到windows的vim的安装目录下对应的文件夹。并将.vimrc重命名为\_vimrc,覆盖掉原有的\_vimrc文件。

- 在\_vimrc中添加以下命令以支持中文编码：    
```
set encoding=utf-8
set termencoding=utf-8
set fileencoding=utf-8
set fileencodings=ucs-bom,utf-8,chinese,cp936
```

- 修改\_vimrc中迁移后产生的个别路径错误问题。

配置好后的vim是[这样](http://pan.baidu.com/s/1kpsLc)的。   

直接打开vim时依然会出现乱码，但是右键一个文件以vim打开时则不会，以后有时间再完善一下。


