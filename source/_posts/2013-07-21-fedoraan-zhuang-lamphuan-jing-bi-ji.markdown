---
layout: post
title: "linux安装lamp环境笔记"
date: 2013-07-21 11:20
comments: true
categories: 
---
前期工作
---
**第1步**： 在终端输入
```
$ sudo yum update  # 更新系统
```
**第2步**：下载apache,mysql,apr,apr-util,phpMyAdmin,php,pcre(非必需）安装包  

<!-- more -->

**第3步**： 
```
$ sudo yum install gcc gcc-c++ libxml2 libxml2-devel pcre pcre-devel #安装依赖软件包
```
**第4步**：tar -zxvf 解压apache,mysql,php,pcre 等压缩包， sudo mv 移动到/usr/local目录下  
```
$ sudo cd /usr/local/pcre;./configure --prefix=/usr/local/pcre #安装pcre
```
（非必需，如果安装apache时显示未安装pcre可按照此法安装）  
**第5步**： cd httpd/srclib/ 解压apr,apr-util 到目录下，修改文件名（不带版本号，如apr-util）

一、安装apache:
---
```
$ cd /usr/local/httpd  # 进入安装目录  
$ sudo ./configure --prefix=/usr/local/apache2 --enable-so --with-mpm=worker --with-pcre=/usr/local/pcre --with-included-apr # 配置（注意./configure的/前有个小点，若没有按照上文安装pcre，可去掉--with-pcre后面的路径）
$ sudo make
$ sudo make install
 #(--prefix指定安装目录，配置文件在/usr/local/apache2/conf目录下，--enable-so启动加载共享模块的功能，--with-mpm=worker吿诉apache使用多线程化多处理模块worker.)
```
**启动apache**:  
```
$ sudo /usr/local/apache2/bin/httpd -k start | stop
```
 或 
```
$ sudo /usr/local/apache2/bin/apachectl start | stop
```
(可用ps -e | grep httpd 查看apache是否已开启）

二、 安装mysql：
---
```
$ cd /usr/local
$ sudo ln -s /usr/local/mysql-××× mysql #链接源码到一个文件夹
$ cd mysql
$ scripts/mysql_install_db --user=[username]    #（username为自己的用户名，中括号不用打进去，下同）
$ sudo chown -R root .     #（注意末尾有个小点）
$ sudo chown -R [username] data
$ sudo chgrp -R [username] .
```
**启动mysql**:  
```
 sudo /usr/local/mysql/bin/mysqld_safe --user=[username] &
```
**设置开机启动Mysql**： 
```
$ sudo cp support-files/mysql.server /etc/init.d/[username]
$ sudo cp support-files/mysqld_multi.server /etc/init.d/[username]
$ sudo chmod +x /etc/init.d/[username]
$ sudo chkconfig --add [username]
```
**测试**：
```
$ sudo /usr/local/mysql/bin/mysqladmin -u root -p version
$ sudo /usr/local/mysql/bin/mysqlshow
```
**修改密码，删除匿名用户**：
```
$ sudo /usr/local/mysql/bin/mysqladmin -u root -p password [password]
$ sudo /usr/local/mysql/bin/mysql -u root -p
```
[password]可以不输入，上面两个命令是修改密码用，此时进入mysql命令界面  
```
mysql> delete from mysql.user where host='localhost' and user='';
mysql> flush privileges;
```
三、安装php
---
```
$ cd /usr/local/php-5__
$ sudo ./configure --prefix=/usr/local/php --with-apxs2=/usr/local/apache2/bin/apxs --with-mysql=/usr/local/mysql
$ make
$ sudo make install
$ sudo cp /usr/local/php___/php.ini-__ /usr/local/php/lib/php.ini  
```
修改httpd.conf 添加AddType application/x-httpd-php .php  
在DirectoryIndex index.html 后加入index.php  
**重启apache**:
```
sudo /usr/local/apache2/bin/apachectl restart
```
四、安装phpMyAdmin:
---
```
tar -zxvf phpMyAdmin___ -C /usr/local/apache2/htdocs
```
到此全部完成





Q1
---
如果测试mysql时没有显示版本信息等界面，而是显示Can't connect to local MySQL server through socket '/tmp/mysql.sock'(2)则极有可能是mysql没有安装成功。解决办法是删除系统自带的mysql服务，
```
$ rpm -qa | grep mysql
```
若有输出结果，则直接用rpm -e删除之，再输入
```
$ chkconfig --list | grep -i mysql
```
检查mysql服务，有的话再删除之
```
$ sudo chkconfig --del mysql
```
然后按照安装mysql的步骤再安装一次即可。

