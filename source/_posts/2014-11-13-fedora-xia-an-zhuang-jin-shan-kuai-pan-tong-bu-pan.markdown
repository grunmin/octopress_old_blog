---
layout: post
title: "Fedora 下安装金山快盘同步盘"
date: 2014-11-13 23:07
comments: true
categories: 技术
tags: linux
---
金山快盘推出了linux系统下的同步盘客户端，但是很遗憾没有红帽系的rpm安装包。尽管如此，我们可以通过一些操作在fedora下安装（mate桌面）（其他红帽系的没有尝试）

<!--more-->

  - 进入[官网](http://www.ubuntukylin.com/applications/showimg.php?lang=cn&id=21) 下载ubuntu版本的客户端（注意系统位数）
  - 解压deb包，可看到`DEBIAN`,`etc`,`usr`,`opt`这几个文件夹（如只看到data.tar压缩包，则解压这个压缩包，可看到这几个文件夹）
  - 进入解压后文件夹根目录，将`usr`,`opt`文件夹复制到系统根目录下
```
# cp -r usr /
# cp -r opt /
```
  - 安装库文件
```
# yum install libqxt
# yum install boost-iostreams
```
需要什么库文件可以通过在终端启动快盘时显示的错误信息排查。如果不是这两个的话也是正常情况。

  - 创建链接
```
# ln /usr/bin/kuaipan4uk /home/yourhomedirectory/anywhereyoulike
```
此时双击该链接即可启动快盘
