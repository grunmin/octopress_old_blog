---
layout: post
title: "fedora 下调节外放音量和屏幕亮度"
date: 2014-03-13 00:43
comments: true
categories: linux
tags: linux
---
###声音
fedora系统下笔记本外放功能可能有受到一些限制。接耳机的时候声音是正常的，但是拔下耳机后电脑并不能外放声音。网上搜到的方法是：  

<!-- more -->


终端运行alsamixer->按F6键，选中对应硬件，应该会出现这样的画面![](https://lh4.googleusercontent.com/-Pj4Qb92Mz3M/UyCOC4GBLFI/AAAAAAAAAJ0/IZSLgP3Xxqs/w614-h346-no/2014-03-13-002941_614x346_scrot.png)，调高第三条的音量即可实现外放。

### 显示
1、可用命令行`xgamma -gamma X`暂时调节笔记本的屏幕亮度,X的值在1以下。

2、`/sys/class/backlight/intel_backlight/brightness`是调节屏幕亮度的，文件中的数字即代表亮度。
用vim编辑器改是不可以的，因此用重定向的方法：
```
# echo 100 > /sys/class/backlight/intel_backlight/brightness
```
数值大概在100到200之间（我的默认是187,后改为100）。

