<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-05-17
title: rsync+inotify实现多台web数据动态同步
tags: rsync,inotify
images: 
category: Linux
status: publish
summary: linux下为了数据安全或者网站同步镜像，不得不考虑一些实时备份的问题，linux下通过rsync+inotify 实现数据实时备份配置过程记录下来，防止遗忘配置过程记录下来，防止遗忘！
-->

linux下为了数据安全或者网站同步镜像，不得不考虑一些实时备份的问题，linux下通过rsync+inotify 实现数据实时备份配置过程记录下来，防止遗忘配置过程记录下来，防止遗忘！  

![image](http://files.jb51.net/file_images/article/201309/20130908114215.gif)  

### 一、配置客服端

1. 首先，检查rsync是否安装,如果没有需要手动安装

```
#检查是否安装过rsync，或者使用命令  rpm -qa|grep rsync 也可以
whereis rsync

#如果结果类似下面这张情况，则已经安装
rsync: /usr/bin/rsync /usr/share/man/man1/rsync.1.gz
#如果结果是,那么说明当前服务器还没有安装过rsync
rsync:

#使用yum安装rsync
yum install rsync
```
2. 创建rsyncd服务的配置文件  
默认安装后，在/etc目录下，并不存在rsyncd目录，需要手动创建配置文件目录

```
mkdir /etc/rsyncd
```
在/etc/rsyncd目录下创建如下文件

```
touch /etc/rsyncd/rsyncd.conf          #主配置文件
touch /etc/rsyncd/rsyncd.secrets       #用户名密码文件，一组用户一行，用户名和密码使用 : 分割
touch /etc/rsyncd/rsyncd.motd          #非必须，连接上rsyncd显示的欢迎信息，此文件可不创建
```

编辑主配置文件 rsyncd.conf

```
uid = root
gid = root
port = 873 							# post rsync使用的端口号  也是默认端口号 www.jbxue.com
hosts allow = 192.168.0.5          	# allow hosts ip 应许的ip访问，也可以设置为ip段
max connections = 5					# 客户端最大连接数
timeout = 300 						# 超时时间

#日志相关
pid file = /var/run/rsyncd.pid
lock file = /var/run/rsync.lock
log file = /var/log/rsyncd.log
#motd file = /etc/rsyncd.motd

[web1]
path =/data/www  					# 客服端与rsync服务端同步的文件路径
comment = from 192.168.10.5  		# 解释
read only = no
list = no							# 是否允许列出模块里的内容
exclude = test1/ test2/				# 排除目录，多个之间使用空格隔开
auth users =www  					# 配置登陆名称
secrets file = /etc/rsync.passwd  	# 配置用户名密码文件
```
配置 rsyncd.secrets  
rsync.passwd内容如下：  
www:abcdefg  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#www登陆用户名  abcdefg是登陆密码（密码自定义）  
```
chmod 0600 /etc/rsyncd/rsyncd.secrets
```
必须注意的是，**rsyncd服务的密码文件权限必须是600！**  

启动rsync，启动后使用netstat查看，会发现系统已启动873端口 

```
#默认配置文件是/etc/rsyncd.conf，所以需要显式的指定配置文件
/usr/bin/rsync --daemon --config=/etc/rsyncd/rsyncd.conf
#假设使用putty，xshell终端操作,保证终端断开进程仍然执行
nohup /usr/bin/rsync --daemon --config=/etc/rsyncd/rsyncd.conf
```
为了保证开机时自动启动，需要手动加上面的命令(/usr/bin/rsync --daemon --config=/etc/rsyncd/rsyncd.conf)加入 /etc/rc.local 文件中  

```
echo "rsync --daemon --config=/etc/rsync.conf" >>/etc/rc.local 
```
如果服务器开启了防火墙，必须保证端口能穿过防火墙

```
iptables -A INPUT -p tcp -m state --state NEW  -m tcp --dport 873 -j ACCEPT
```
关闭rsync

```
killall rsync 
```

客服端配置完成！

### 二、配置服务端
安装rsync（仅安装即可，不需配置）  
在/etc下创建目录  rsyncd

```
mkdir /etc/rsyncd
```
在/etc/rsyncd目录下创建如下文件

```
touch rsync-client.passwd 
```
写入客户端同步的密码

```
echo "abcdefg" > /etc/rsyncd/rsync-client.passwd # 跟上面设置一样即可，此处不需要写用户名
```
修改权限
```
chmod 600 /etc/rsyncd/rsync-client.passwd 
```

安装inotify 下载地址：https://github.com/rvoicilas/inotify-tools/wiki/ 

```
cd /usr/local/src 
wget http://cloud.github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz 
tar zxvf inotify-tools-3.14.tar.gz 
cd inotify-tools-3.14 
./configure 
make 
make install 
```
创建启动脚本

```
vi /etc/rsyncd/rsync-web.sh

#!/bin/sh 
SRC=/data/www/www.niaoyun.com
DES=www.niaoyun.com 
BACK01=192.168.0.6
BACK02=192.168.0.7
BACK03=192.168.0.8
USER=www 
/usr/local/bin/inotifywait -mrq -e create,move,delete,modify $SRC | while read D E F 
do
rsync -avzP --delete --password-file=/etc/rsyncd/rsync-client.passwd $SRC $USER@$BACK01::$DES
rsync -avzP --delete --password-file=/etc/rsyncd/rsync-client.passwd $SRC $USER@$BACK02::$DES
rsync -avzP --delete --password-file=/etc/rsyncd/rsync-client.passwd $SRC $USER@$BACK03::$DES
done 
```
增加权限
```
chmod +x /etc/rsync-web.sh 
```
启动脚本

```
nohup /etc/rsync-web.sh & #必须使用nohup放入后台执行，否则关闭终端后此脚本进程会自动结束 /etc/rsync-web.sh & 
```
关闭脚本 

```
sudo pkill rsync 
sudo pkill inotifywait
```


























