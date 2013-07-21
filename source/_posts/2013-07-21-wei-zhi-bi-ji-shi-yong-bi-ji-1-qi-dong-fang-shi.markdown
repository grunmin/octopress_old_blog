---
layout: post
title: "为知笔记使用笔记(1)-启动方式"
date: 2013-07-21 10:40
comments: true
categories: 
---
###**为知笔记下载：**
[为知笔记下载](http://www.wiz.cn/download.html)  

解压包到自己名下的文件夹，启动图标在bin文件夹里。

###方式1 创建变量
在加环境变量后可以直接在命令行启动
```
# vi /etc/profile  
# export PATH="$PATH:/home/username/path/wiznote/bin/"  
# source /etc/profile #重新加载环境变量  
```
然后就可以在命令行下直接输入wiznote启动。
  
<!-- more -->

###方式2 使用alias
其实我们可以在命令行直接输入/path/to/wiznote/bin/wiznote来启动，不过每次都需要输入很长的一段命令，因此可以用alias来使命令更简单  
```
$ vi .bashrc  #打开bash配置文件  
```
在文档末尾添加一行
```
alias note="/path/to/wiznote/bin/wiznote"
```
保存，重新启动终端模拟器，输入note即可启动  
（/path/to…… 需要根据自己的解压目录具体修改）

###方式3 创建桌面快捷方式
直接复制bin/wiznote到桌面是不能正常使用wiznote的，需要为其创建一个链接，具体如下：
在终端输入  
```
$ ln -s /path/to/wiznote/bin/wiznote /path/want/to/locate
```
第一个path是原来wiznote的目录，第二个path是要安放快捷方式的地方。  
直接双击图标即可启动。

###方式4 添加到快捷启动栏
```
# vi /usr/share/applications/wiznote.desktop
```
复制一下代码到文件中：
```
[Desktop Entry]  
Name=wiznote  
GenericName=cloud note-taking application  
Comment=taking note and manage information  
Exec=/pate/to/wiznote/bin/wiznote  
Terminal=false  
Type=Application  
StartupNotify=true  
Icon=/path/toznote/share/wiznote/skins/wiznote64.png  
Categories=KDE;QT;Utility;  
```
其实以上代码可在/path/to/wiznote/share/applications/wiznote中找到。  
Name是启动栏里显示的名称，Exec是打开路径，Icon是图标路径，Categories是归类，可以填Network.
