---
layout: post
title: "linux服务器本地和百度云备份脚本小试"
date: 2014-11-13 23:06
comments: true
categories: 技术
tags: linux
---
###本地单文件上传脚本，命名uf
这是在本机上做的测试，利用`bpcs_uploader`脚本实现，只是进行简单的封装，自动完善云端文件路径。

技术要点：使用`dirname`获取文件所在目录，使用`pwd`获取文件完整路径，并作为云端文件路径。

<!--more-->

```
#!/bin/bash
cur_dir=$(cd "$(dirname "$1")"; pwd)
name=$(basename "$1")
/home/grm/bin/bpcs_uploader/bpcs_uploader.php upload $1 AWIN$cur_dir/$name
```
###本地文件夹上传脚本，命名ud
`bpcs_uploader`脚本只能实现单个文件上传，用此脚本可以实现目录批量上传。

技术要点：通过`find`命令输出目录下所有文件，用`xargs -t -n1`实现单个输出，从而可以遍历目录下所有文件，并作为参数逐次赋予脚本`uf`，通过不断调用脚本`uf`实现批量上传。
```
#!/bin/bash
find $1 -name '*.*' |xargs -t -n1 /home/grm/bin/uf
```

###服务器数据库每日备份脚本，命名backupday.sh(改自鸟哥的linux私房菜)

技术要点：基本都是常规操作，注意`find`命令`-mtime`参数的使用
```
#!/bin/bash
# =========================================================
# 请输入，你想让备份数据放置到那个独立的目录去
basedir=/backup/daily/

# =========================================================
PATH=/bin:/usr/bin:/sbin:/usr/sbin; export PATH
export LANG=C
basefile1=$basedir/mysql.$(date +%Y-%m-%d).tar.bz2
basefile2=$basedir/cgi-bin.$(date +%Y-%m-%d).tar.bz2
[ ! -d "$basedir" ] && mkdir $basedir

# 1. MysQL (数据库目录在 /var/lib/mysql)
cd /var/lib
  tar -jpc -f $basefile1 mysql 

# 2. 定期删除旧备份
DAYS=30
find $basedir -name "mysql*" -type f -mtime +$DAYS -exec rm {} \;
```

###代码及其他配置每周备份脚本，命名为backupweek.sh
```
#!/bin/bash
# ====================================================================
# 使用者参数输入位置：
# basedir=你用来储存此脚本所预计备份的数据之目录(请独立文件系统)
basedir=/backup/weekly  

# ====================================================================
# 底下请不要修改了！用默认值即可！
PATH=/bin:/usr/bin:/sbin:/usr/sbin; export PATH
export LANG=C

D=$(date +"%Y-%m-%d")

# 配置要备份的服务的配置档，以及备份的目录
postfixd=$basedir/postfix
vsftpd=$basedir/vsftp
sshd=$basedir/ssh
wwwd=$basedir/www
others=$basedir/others
userinfod=$basedir/userinfo
# 判断目录是否存在，若不存在则予以创建。
for dirs in $postfixd $vsftpd $sshd $wwwd $others $userinfod
do
    [ ! -d "$dirs" ] && mkdir -p $dirs
done

# 1. 将系统主要的服务之配置档分别备份下来，同时也备份 /etc 全部。

cd /etc/
    tar -jpc -f $vsftpd/vsftpd.$D.tar.bz2 vsftpd
cd /etc/
    tar -jpc -f $sshd/sshd.$D.tar.bz2 sshd ssh
cd /etc/
    tar -jpc -f $wwwd/httpd.$D.tar.bz2 httpd
cd /var/www
  tar -jpc -f $wwwd/html.$D.tar.bz2    html 
cd /
  tar -jpc -f $others/etc.$D.tar.bz2   etc

# 2. 关於使用者参数方面
cp -a /etc/{passwd,shadow,group}    $userinfod
cd /var/spool
  tar -jpc -f $userinfod/mail.$D.tar.bz2   mail
cd /
  tar -jpc -f $userinfod/home.$D.tar.bz2   home
cd /var/spool
  tar -jpc -f $userinfod/cron.$D.tar.bz2   cron at

# 3. 定期删除旧备份
DAYS=30
find $vsftpd -name "vsftpd*" -type f -mtime +$DAYS -exec rm {} \;
find $sshd -name "sshd*" -type f -mtime +$DAYS -exec rm {} \;
find $wwwd -name "ht*" -type f -mtime +$DAYS -exec rm {} \;
find $others -name "etc*" -type f -mtime +$DAYS -exec rm {} \;
find $userinfod -name "cron*" -type f -mtime +$DAYS -exec rm {} \;
find $userinfod -name "home*" -type f -mtime +$DAYS -exec rm {} \;
find $userinfod -name "mail*" -type f -mtime +$DAYS -exec rm {} \;
```

###自动上传脚本auto_upload_daily.sh
其中upload.sh的代码与本地脚本`uf`相同。简言之，脚本`uf`是云备份的基础。
```
#!/bin/bash
LOCAL_DATA=/backup/daily
MYSQL_BACKUP=mysql.$(date +"%Y-%m-%d").tar.bz2

upload.sh $LOCAL_DATA/$MYSQL_BACKUP
```

###自动上传脚本auto_upload_weekly.sh
```
#!/bin/bash
LOCAL_DATA=/backup/weekly
D=$(date +"%Y-%m-%d")

HTTP=www/httpd.$D.tar.bz2
HTML=www/html.$D.tar.bz2
ETC=others/etc.$D.tar.bz2
HOM=userinfo/home.$D.tar.bz2
MAIL=userinfo/mail.$D.tar.bz2
PASSWD=userinfo/passwd.$D.tar.bz2
SHADOW=userinfo/shadow.$D.tar.bz2
SSHD=ssh/sshd.$D.tar.bz2
VSFTPD=vsftpd/vsftpd.$D.tar.bz2
CRONA=userinfo/cron.$D.tar.bz2

upload.sh $LOCAL_DATA/$HTTP
upload.sh $LOCAL_DATA/$HTML
upload.sh $LOCAL_DATA/$ETC
upload.sh $LOCAL_DATA/$HOM
upload.sh $LOCAL_DATA/$MAIL
upload.sh $LOCAL_DATA/$PASSWD
upload.sh $LOCAL_DATA/$SHADOW
upload.sh $LOCAL_DATA/$CRONA
upload.sh $LOCAL_DATA/$SSHD
upload.sh $LOCAL_DATA/$VSFTPD
```

###最后，再启动定时任务
为防止网络出现问题导致上传失败，重复了3次上传操作
```
# crontab -l
01 1 * * * /bin/backupday.sh 2>>/backup/errors.log
20 1 * * 0 /bin/backupwk.sh 2>>/backup/errors.log
01 2 * * * /bin/auto_upload_daily.sh 2>>/backup/errors.log
01 4 * * * /bin/auto_upload_daily.sh 2>>/backup/errors.log
01 6 * * * /bin/auto_upload_daily.sh 2>>/backup/errors.log
20 2 * * 0 /bin/auto_upload_weekly.sh 2>>/backup/errors.log
20 4 * * 0 /bin/auto_upload_weekly.sh 2>>/backup/errors.log
20 6 * * 0 /bin/auto_upload_weekly.sh 2>>/backup/errors.log
```
