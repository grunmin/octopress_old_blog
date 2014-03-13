---
layout: post
title: "apache服务器个人主页的设置"
date: 2014-01-02 16:16
comments: true
categories: linux
tags: linux
---

- 查看apache版本
```
httpd -v
```
我的版本是2.4.6，个人主页相关的配置文件在  
`/etc/httpd/conf.modules.d/00-base.conf`  
和 
`etc/httpd/conf.d/userdir.conf`  

<!--more-->

- 查看`/etc/httpd/conf.modules.d/00-base.conf`，必须有`userdir_module`模块，如：
```
$ cat /etc/httpd/conf.modules.d/00-base.conf | grep userdir
LoadModule userdir_module modules/mod_userdir.so
```

- 进入`/etc/httpd/conf.d/userdir.conf`设置，将第17行的`UserDir disabled`加上注释并取消24行`UserDir public`的注释(可以更改UserDir后面文件夹的名字，比如`UserDir www`)

- 进入个人家目录，创建一个文件夹www和测试页面
```
mkdir ~/www
cd ~/www
echo "Test home dir" >> index.html
```
- 重启服务器，用浏览器打开`http://localhost/~username`，应该会出现403页面。
```
client denied by server configuration
```

将selinux关闭，查看是否可以访问。

- 如果不能访问，添加下面的代码到`/etc/httpd/conf/httpd.conf`文件中

```
 #将name和username按情况替换掉
<Directory "/home/username/www">
    AllowOverride None
    Options Indexes FollowSymLinks
    Require all granted
</Directory>
```

这里要注意的是，apache2.4版本已经取消了
```
    Order Deny,Allow
    Allow from All
```
的使用，而采用
```
    Require all granted
```
来设置对目录的访问。


- 设置别名 
```
Alias /name/ "/home/usename/www/" 
```
到此个人目录可以使用，将代码放到~/www目录下吧^_^。

>如果服务器总是显示403的话，检查以下你的家目录是否具有执行权限，修改为755即可。
