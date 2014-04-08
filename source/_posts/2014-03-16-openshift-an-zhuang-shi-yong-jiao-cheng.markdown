---
layout: post
title: "Openshift 安装使用教程"
date: 2014-03-16 21:09
comments: true
categories: 技术
tags: linux 工具
---
声明：配置环境是fedora 20，这里是[官方文档](https://access.redhat.com/site/documentation/en-US/OpenShift_Online/2.0/html/User_Guide/index.html)，找文档是解决问题最有效的方式。

<!-- more -->

###介绍
>OpenShift是红帽公司推出的一个云计算服务平台，开发人员可以用它来构建和发布web应用。Openshift广泛支持多种编程语言和框架，如Java，Ruby和PHP等。另外它还提供了多种集成开发工具如Eclipse integration，JBoss Developer Studio和 Jenkins等。OpenShift 基于一个开源生态系统为移动应用，数据库服务等，提供支持。
——来自百度百科

简言之，就是一个面向开源开发人员开放的平台即服务(PaaS)。

使用openshift首先需要一个账户，注册十分简单，不在此赘述。
配置自己的openshift可以通过几种方式，比如web端，命令行。web端适合刚刚接触的新手，建议新用户多点点链接，熟悉一下大体的使用方法和功能。当准备好创建应用时即往下看。本文是介绍命令行下的配置，毕竟修改代码什么的都需要在本地修改后再提交。而openshift的客户端就是基于命令行的。

###安装
#####安装命令行客户端
```
sudo yum install rubygem-rhc -y
```
#####初始化设置
```
sudo rhc setup
```
此时需要输入帐号和密码，即为自己在openshift注册时的帐号。

令人费解的是使用我的本地账户运行`rhc`显示找不到命令，切换为root运行也是如此。只能使用`sudo rhc`的方式，后面运行`git push`也是如此，必须是`sudo git push`。如果你知道原因，烦请不吝赐教。
#####使用gem更新rhc
```
sudo gem update rhc
```
然后看看自己的账户
```
sudo rhc account
```
在安装rhc的时候本地用户已和云端绑定，ssh公钥也导入了，因此之后的操作一般不需要再作验证。
#####查看自己的app
```
sudo rhc apps
```
可以看到自己账户下有多少个app（免费账户最多3个），应用名，git仓库地址，主机地址，绑定的域名，以及模块，数据库版本，数据库用户和密码等等。比如我的wordpress（数据库账户和密码被我屏蔽）。
```
blog @ http://blog-grunmin.rhcloud.com/ (uuid: 5325520fe0b8cd3a830009ff)
------------------------------------------------------------------------
  Domain:          grunmin
  Created:         3:26 PM
  Gears:           1 (defaults to small)
  Git URL:         ssh://5325520fe0b8cd3a830009ff@blog-grunmin.rhcloud.com/~/git/blog.git/
  Initial Git URL: https://github.com/openshift/wordpress-example.git
  SSH:             5325520fe0b8cd3a830009ff@blog-grunmin.rhcloud.com
  Deployment:      auto (on git push)
  Aliases:         wp.guorunmin.cn

  mysql-5.1 (MySQL 5.1)
  ---------------------
    Gears:          Located with php-5.3
    Connection URL: mysql://$OPENSHIFT_MYSQL_DB_HOST:$OPENSHIFT_MYSQL_DB_PORT/
    Database Name:  blog
    Password:       ********
    Username:       ******

  php-5.3 (PHP 5.3)
  -----------------
    Gears: Located with mysql-5.1
```
###创建应用
#####创建
看别人的东西干过瘾，不如自己创建一个，创建的方法也很简单，只需一个命令
```
sudo rhc app create AppName 
```
Openshift 支持Java，Ruby，Node.js，PHP，Perl和Python，在AppName 后面可以加其他参数，例如php应用是：
```
sudo rhc app create AppName php-5.4
```
有一种是diy模式，即自行搭建语言环境的，面对一台陌生的机器，我没有任何信心能完成这样的工作^_^。刚开始的时候，我找不到CI框架的环境（有ZF），所以选择新建空应用，以为我自己的代码可以在服务器上运行，想想还是比较天真哈哈哈
如果是安装环境了，改代码的之前需要将仓库克隆到本地。如果新建空应用，那么rhc会自动将仓库克隆下来。

更多参数可以参见[官方文档](https://access.redhat.com/site/documentation/en-US/OpenShift_Online/2.0/html/User_Guide/Creating_an_Application5.html)，不用担心看不懂英文，我翻了一遍，文档大体都能看懂。

#####添加mysql和phpmyadmin模块
既然是php应用，mysql和phpmyadmin怎么能少呢！让我们添加一下
```
sudo rhc cartridge add mysql-5.1 -a AppName
sudo rhc cartridge add phpmyadmin -a AppName
```
此时新建的应用信息大概就是你看到的上面那个。建议保存一下有用的信息，比如phpmyadmin的地址，用户名和密码，尤其是使用web端创建的同学\*_\*

*安装wordpress后还需登陆网站添加安装配置信息*

如果要删除应用，执行
```
sudo rhc app delete AppName
```


###开始码字
如果安装的是wordpress这些成熟的应用，因为它提供了管理界面，大概不用去修改代码。如果是自己开发的应用呢？Openshift支持git的方式修改云端上的代码。大体流程应该是这样：
克隆代码到本地->进入应用目录->进入代码目录->修改代码->git add,commit,push->服务器上的应用关闭->一系列编译布置->应用重启->完成

克隆操作
```
sudo rhc git-clone AppName
```
或者知道仓库地址，像这样
```
sudo git clone ssh://53257e56e0b8cd671500019b@app-grunmin.rhcloud.com/~/git/app.git/
```
php应用根目录下默认有两个文件夹，.git和.openshift。（安装wordpress时候还有另外三个目录：libs,misc,php，但是我找不到wordpress的代码）.openshift目录的作用官网的说明挺详细，我只知道是存放git动作触发的脚本文件，没有深入研究。根目录下还有index.php文件，就是登陆AppName-AccountName.rhcloud.com时看到的页面。这样的设计应该很明朗了，我们直接将代码放在应用根目录下即可。或许你可以写个测试文件看看php的环境，比如"welcome.php"
```
<?php
         phpinfo();
?>
```

另外我们可以ssh进去主机看看里面的文件。如果想了解的话还是自己找找资料吧，比如[openshift用ssh登陆后的目录结构](http://www.dashashi.com/index.php/2013/01/1435)。

###访问
虽说是否被墙是判断一个服务好坏的标准，不过眼看着这么好的服务被墙还是挺让人窝火。因此我也希望我们能够好好利用openshift，不要见缝插针，浪费资源。

如果是博客，那么有一个自己的域名当然比较好。如果不是的话，为了避免被墙，建议也绑定一个，虽然不一定能起作用。绑定域名，也是一个命令搞定：
```
sudo rhc alias add AppName YourDomain
```
当然，你需要在域名提供商那里添加一个CNAME记录，指向你的openshift域名。
*因为缓存的原因，域名解析不会立刻生效。*

或者使用https的方式访问。

其他跨栏的姿势很多，不赘述。


参考：
[openshift使用方法介绍](http://www.live-in.org/archives/1818.html)

[openshift用ssh登陆后的目录结构](http://www.live-in.org/archives/1818.html)

