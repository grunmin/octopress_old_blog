---
layout: post
title: "fedora 下调节外放音量和屏幕亮度"
date: 2014-03-13 00:43
comments: true
categories: 技术
tags: linux
---
###声音
fedora系统下笔记本外放功能可能有受到一些限制。接耳机的时候声音是正常的，但是拔下耳机后电脑并不能外放声音。网上搜到的方法是：  

<!-- more -->


终端运行alsamixer->按F6键，选中对应硬件，应该会出现这样的画面![](https://lh4.googleusercontent.com/-Pj4Qb92Mz3M/UyCOC4GBLFI/AAAAAAAAAJ0/IZSLgP3Xxqs/w614-h346-no/2014-03-13-002941_614x346_scrot.png)，调高第三条的音量即可实现外放。

### 显示
1、可用命令行`xgamma -gamma X`暂时调节笔记本的屏幕亮度,X的值在1以下。

2、如果用英特尔集成显卡，则`/sys/class/backlight/intel_backlight/brightness`是调节屏幕亮度的，文件中的数字即代表亮度。
用vim编辑器改是不可以的，因此用重定向的方法：
```
# echo 100 > /sys/class/backlight/intel_backlight/brightness
```
数值大概在100到200之间（我的默认是187,后改为100）。

如果用NVIDIA显卡，则存在`/sys/class/backlight/acpi_video0/brightness`这个文件，更改方法如上。

这两个方法都不能永久性地更改亮度。不过我们可以通过脚本的方式，让计算机启动的时候执行这些命令，以达到开机即调整亮度的需求。  

在/etc/rc.d/init.d/目录下，新建一脚本，比如命名为brightness，写入以下内容
```
#add for chkconfig
#chkconfig:2345 70 30
#description:modify brightness
#processname:brightness
xgamma -gamma 0.7
echo 50 > /sys/class/backlight/intel_backlight/brightness
echo 50 > /sys/class/backlight/acpi_video0/brightness
```
其中2345表示脚本在哪一运行级别生效，70和30表示开机/关机时启动/关闭的顺序，数字越大越靠后。processname即为服务的名称，作标识用。

给脚本可执行权限
```
chmod +x brightness
```
将脚本添加入服务控制列表
```
chkconfig --add brightness
```
此时用`chkconfig --list`可查看brightness服务在2345运行级别里是默认开启的。

