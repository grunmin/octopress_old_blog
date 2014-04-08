---
layout: post
title: "sublime text 2 下的Markdown写作"
date: 2014-03-12 22:48
comments: true
categories: 技术
tags: windows 工具
---

作为Windows/Mac/Linux下强大的文本编辑器，st提供了对Markdown语言的支持。通过设置可实现markdown预览和转换功能。而本文介绍的`Markdown Preview`支持Mathjax语法和目录自动生成。(Windows下）

<!-- more -->


##安装Package Control
安装包控制扩展可以方便地为st添加拓展。
打开st，按下组合键`Control + \``，出现控制台，输入
```
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```
当看到代码最后一行提示的时候说明安装成功，此时重启st，可在`Preferences -> Package Settings`看到`Package Control`。

##安装markdown preview
按下键`Ctrl+Shift+p`调出命令面板，找到`Package Control: install Pakage`这一项。搜索markdown preview，点击安装。

##使用
Markdown Preview较常用的功能是`preview in browser`和`Export HTML in Sublime Text`，前者可以在浏览器看到预览效果，后者可将markdown保存为html文件。

`preview in browser`据称是实时的，但是实践上还是需要在st保存，然后浏览器刷新才能看到新的效果，好在markdown写得多的话也不需要每敲一行看一次效果。

##快捷键
st支持自定义快捷键，`markdown preview`默认没有快捷键，我们可以自己为`preview in browser`设置快捷键。方法是在`Preferences -> Key Bindings User`打开的文件的中括号中添加以下代码(可在Key Bindings Default找到格式)：
```
 { "keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"} }
```
"alt+m"可设置为自己喜欢的按键。

##设置语法高亮和mathjax支持
在`Preferences ->Package Settings->Markdown Preview->Setting Default`中的第31行和36行找到
```
/*
       Enable or not mathjax support.
    */
    "enable_mathjax": false,

    /*
        Enable or not highlight.js support for syntax highlighting.
    */
    "enable_highlight": false,
```
将 两个false改为true即可。
语法高亮跟编辑器的主题有关，可以在`Preferences ->Color Scheme`找自己喜欢的主题。
关于目录生成，只要文章是按照markdown语法写作的。在需要生成目录的地方写
```
[TOC]
```
即可。
>如果你这里没有看到目录而只是看到代码，说明简书不支持目录自动生成哈哈



##打印成pdf
将markdown转换为pdf应该有很多种方法的。我没有再折腾，直接用谷歌浏览器虚拟打印功能生成。
利用`Markdown Preview`的`Preview in Browser`功能可以在浏览器上看到htm效果。在页面`右键->打印->另存为pdf->调节页边距`即可将pdf文件下载下来。

