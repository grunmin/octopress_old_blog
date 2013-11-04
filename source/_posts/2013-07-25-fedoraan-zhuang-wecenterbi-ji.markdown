---
layout: post
title: "linux安装wecenter笔记"
date: 2013-07-25 20:32
comments: true
categories: linux
---
*此文是前文配置lamp的延伸*  
在前面的一片文章里，用源码包安装的方式搭建了lamp环境，wecenter需要服务器支持mysqli或者pdo模块,支持curl,GD图像处理库，原来安装lamp过程中并无相关方面的配置，鼓捣了半天之后其他条件都已满足，唯独GD图像处理库不能安装。  
<!-- more -->
谷歌了许多结果，一一尝试，一一失败，最后看到[这篇文章](http://tech.soft6.com/665/16/78687.html)，果断把之前编好的php删除了按照文中方法重装一遍，居然成功了！

现在从零开始记录一下安装过程。

按照上一篇笔记配置好lamp环境，php先不安装。
把下载下来的wecenter里的UPLOAD复制到/usr/local/apache2/htdocs/里（即文件根目录），打开浏览器，输入地址`http://localhost/UPLOAD/install`，进入安装界面。在我的机子上，一开始不能满足的条件是数据库模块，curl支持，图像处理库和目录权限。  
从最简单的开始，因为后面还会要求UPLOAD下的文件开放权限，因此直接一个  
```
# chmod 777 /usr/local/apache2/htdocs/UPLOAD
```
之后就是折腾了很久的GD，Linux下得GD库安装要比windows麻烦很多。先参照此文：http://tech.soft6.com/665/16/78687.html  
此前的多次实验表明这个是唯一比较靠谱的安装方法（当然不才眼界有限）。装好之后就可以进行下一步了。  
开启curl和mysqli或者pdo支持，可以用如下方法
以curl为例，首先  
```
# cd /usr/local/php-***/ext/curl #进入源码包的curl目录
# /usr/local/php/bin/phpize #运行phpize命令
# ./configure --with-php-config=/usr/local/php/bin/php-config
# make && make install
```
成功后可以看到创建的curl.so文件位于/usr/local/php/lib/php/extension/no-debug-non-zts-*******/,复制curl.so到/usr/local/php/ext下(没有目录则创建一个)，再敲入  
```
# vi /usr/local/php/lib/php.ini​
```
在加载模块(一堆extension处 大概第980行)加上一行extension=curl.so 修改extension_dir=“/usr/local/php/ext” (第819行),重启apache即可。(前面的操作如需立刻看到效果也需重启apache)

[这里](http://blog.csdn.net/grunmin/article/details/9468639)是增加php模块的方法介绍，mysqli可用此法解决。

*注：用rpm包搭建lamp时没有出现上述问题*
