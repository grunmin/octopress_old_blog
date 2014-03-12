---
layout: post
title: "wamp apache无法启动的解决方法"
date: 2014-03-12 22:48
comments: true
categories: windows
tags: windows
---

如题，近日在安装wamp的时候出现了apache无法启动的情况。wamp图标一直显示橙色。网上搜索了许多资料都无效，最后还是我自己发现问题并予以解决了。

<!-- more -->


###VC的问题？
我的电脑是新装的win8.1企业版。因此我怀疑是VC2008和2010没有安装的缘故，因此上官网下载了这两个安装包安装，结果如故。

###没有安装服务？
直接`apache->service->remove service`，再install service ，重新打开wamp，结果如故。

###80端口被占用？
用apache里service的test port 80，显示`Your port 80 is not actually used`。在cmd里用netstat -an 查找，没有一个软件占用80端口。为保险起见修改httpd.conf文件，修改其端口为8080，重启，结果如故。

###netbios解析失败？
打开网络共享中心，点击`本地连接->属性->Internet协议版本->属性->高级->WINS`，勾掉`启用LMHOST`查找。重启服务，结果如故。

###httpd.conf修改后语法错误？
新装的wamp，初始的配置文件怎么可能有语法错误……

于是把wamp卸了又装装了又卸，问题得不到解决。

换另一种方法，自己分开安装wamp。

###失败！
在安装apache的时候显示找不到\*\*\*文件，请检查路径之类的问题，安装程序出错，此举以失败告终。

因此我再装一次wamp，查看httpd.conf，在其中看到的路径中包含了中文名。是不是因为名称的问题导致的呢？我不敢确定，因为同样是中文路径，mysqld服务可以成功启动。

###成功！
接下来的事就简单了，不过也比较刺激。因为为了解决这个问题我搜索了两天，真的只是该死的中文路径名的问题。

这次问题的解决再次提醒我注意之前多次遇到的问题，即windows下中文路径名的问题。即使转移到windows下，也不要忘了linux下良好的命名习惯呀。

