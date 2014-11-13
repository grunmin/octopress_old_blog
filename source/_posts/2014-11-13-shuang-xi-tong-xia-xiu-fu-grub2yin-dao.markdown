---
layout: post
title: "双系统下修复grub2引导"
date: 2014-11-13 22:28
comments: true
categories: 技术
tags: linux
---
我的电脑上装有fedora和windows两个系统。一般装系统的时候windows先装而linxu后装，因为linux能够查找windows系统并为其提供启动引导，而windows不能。因此当需要重装windows的时候需要修复grub2引导。

<!-- more -->

**工具**
fedora的livecd，即安装fedora时使用的安装U盘

##步骤
###调整bios启动顺序，进入livecd
###挂载硬盘
  - 使用`fdisk -l`查找/boot分区所在硬盘，比如我的是/dev/sda8。（在此之前需要运行`su`命令）
  - 挂载根分区`mount /dev/mapper/fedora-root /mnt`
  - 挂载/boot分区`mount /dev/sda8 /mnt/boot`
  - 挂载/dev分区`mount --bind /dev /mnt/dev`
  
###修改根目录
`chroot /mnt`

###修复grub2
  - `grub2-install /dev/sda`
  - `grub2-install /dev/sda --recheck`
  
###更新引导项
`grub2-mkconfig -o /boot/grub2/grub.cfg`

若不执行更新引导项，则启动的时候会显示过去存在的操作系统（此时并不会显示新的操作系统）

进入fedora后，再次执行`grub2-mkconfig -o /boot/grub2/grub.cfg`，则引导项修复正常。

###更改默认引导项
可以通过在`/boot/grub2/grub.cfg`添加`set default=X`（X为数字）来更改默认引导项。X从0开始，例如windows在引导菜单的第4行，则可以设置`set default=3`

但是，这样的修改方法是不推荐的。因为每次执行`grub2-mkconfig -o /boot/grub2/grub.cfg`都会更新`grub.cfg`这个文件。正确的方法是执行`grub2-set-default <标题或名称>`。其中，<标题或名称>用命令`grep menuentry /boot/grub2/grub.cfg`找到，即menuentry后引号内的内容。



