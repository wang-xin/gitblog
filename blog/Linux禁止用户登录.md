<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-03-05
title: Linux禁止用户登录
tags: Linux
images: 
category: Linux
status: publish
summary: Linux禁止用户登录
-->

#### 使用命令usermod

禁止帐号myuser登录
usermod -L myuser 

允许账号登录
usermod -U myuser
#### *修改shell类型*

chsh myuser -s /sbin/nologin  
此时查看/etc/passwd myuser那一行发现发现被改变了

原来为：myuser:x:500:500::/home/myuser:/bin/bash  
此时为：myuser:x:500:500::/home/myuser:/sbin/nologin  
