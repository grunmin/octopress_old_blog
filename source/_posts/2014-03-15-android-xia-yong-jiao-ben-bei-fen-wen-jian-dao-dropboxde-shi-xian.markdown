---
layout: post
title: "android 下用脚本备份文件到dropbox的实现"
date: 2014-03-15 23:28
comments: true
categories: linux android
tags: android
---

前些日子寻找linux下上传文件到云的脚本，以实现远程服务器上资料的云端备份。

<!-- more -->

国内的网盘虽说正逐渐在开放API，但是到实现方便个人云存储应该还需一定的时间，百度云的pcs接口也关闭了（现在重新开放）。这方面国外的Dropbox做得不错，已经有一些相对成熟的应用。比如脚本[Dropbox-Uploader](https://github.com/andreafabrizi/Dropbox-Uploader)。

>Dropbox Uploader is a BASH script which can be used to upload, download, delete, list files (and more!) from Dropbox, an online file sharing, synchronization and backup service.

>It's written in BASH scripting language and only needs cURL.

Dropbox Uploader 是一个bash脚本，提供上传，下载，移动网盘上的文件等功能。通过学习已经实现了远程服务器上的云端备份（2G空间已塞满……准备转投百度云）。

既然是bash脚本，那么怀着linux心的android系统应该也可以使用。虽然现在各种云app充斥眼球，但是相对而言它们都太庞大了。比如百度云app，安装后大小有20+M。我只需要一个能提供上传下载的app，而不是一个集社交，防盗为一体，还总是要后台运行的臃肿app。因此开始着手如何将Dropbox Uploader用在手机上。

我们知道，android下的shell是不完整的，默认是ash。因此我主要做的工作是弄出一个bash的环境，没有什么技术含量，纯粹是一时兴起。

那就开始吧。

1、手机提权

就是root了，关于root的教程网上都有。没有取得root权限，后面的操作都无法执行。对我来说，自己的手机不root就不像是自己的手机。

2、安装busybox，终端模拟器

可以在谷歌市场下载[busybox](https://play.google.com/store/apps/details?id=stericson.busybox&hl=zh_CN)和[终端模拟器](https://play.google.com/store/apps/details?id=jackpal.androidterm&hl=zh_CN)。busybox提供了bash下的一系列常用的命令。比系统自带的强。安装需要root权限，正常安装即可。终端模拟器用来输入命令，也可用[脚本管理器](https://play.google.com/store/apps/details?id=os.tools.scriptmanager)。

3、下载bash，curl，dropbox_uploader.sh

下载[bash](http://pan.baidu.com/s/1i3mhTyd)，[curl](http://pan.baidu.com/s/1lGkZ8)，到/system/bin/目录下。在终端模拟器下输入`bash`应该能进入bash。dropbox_uploader.sh即为Dropbox Uploader的文件名，也放在/system/bin/目录下。

4、其他准备

首先修改Dropbox Uploader文件的第一行，将其sh改为/system/bin/bash，然后新建一个文件夹/tmp,`mkdir /tmp`。

5、创建dropbox应用

使用Dropbox Uploader前需要在Dropbox上创建应用，让该应用具有文件修改权限。具体如何实现可参考[Dropbox Uploader的使用教程](http://teddysun.com/178.html)
Dropbox Uploader的数据传输是加密的，前面的那些设置并没有包含这些。(当时没有找到方法，现在也没有去折腾。因此在使用Dropbox Uploader时需加 `-k`参数，不检查ssl证书。)

6、使用

按照上面的方法设置后，应该就可以用Dropbox Uploader上传文件了。因为在手机上敲打命令不易，因此我们可以另外写个脚本，比如：
```
#!/system/bin/bash
/system/bin/dropbox_uploader.sh -k -s upload $1
```
将脚本命名为u，打开终端模拟器，键入u filename即可将文件上传到dropbox。下载也是一样。


**后记**

这是我寒假在家没有电脑的消遣之举，现在看来这东西实在非常鸡肋，但是当时弄了一个晚上。终于弄成功时那种喜悦难以言表，因此打算回校写篇教程。本文言尽于此，后续不再完善存在的问题。因为我已经找到了完美的替代品，[软件数据线](https://play.google.com/store/apps/details?id=com.damiapp.softdatacable&hl=zh_CN)。



参考引用：
[备份利器Dropbox Uploader](http://teddysun.com/178.html)
