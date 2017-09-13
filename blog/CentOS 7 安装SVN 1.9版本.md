<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-08-15
title: CentOS 7安装1.9版本SVN
tags: Linux, CentOS, svn
images: 
category: Linux
status: publish
summary: CentOS 7安装1.9版本SVN
-->

1.删除已安装的svn
```
yum remove subverson
```

2.设置svn1.9安装源
```
vim /etc/yum.repos.d/wandisco-svn.repo
#输入如下
[WandiscoSVN]
name=Wandisco SVN Repo
baseurl=http://opensource.wandisco.com/centos/7/svn-1.9/RPMS/$basearch/
enabled=1
gpgcheck=0
```

3.执行安装
```
yum clean all
yum install subversion
```

4.验证安装版本
```
svn --version
```


##### 一键安装脚本
```
yum remove subverson
sudo tee /etc/yum.repos.d/wandisco-svn.repo <<-'EOF'
[WandiscoSVN]
name=Wandisco SVN Repo
baseurl=http://opensource.wandisco.com/centos/7/svn-1.9/RPMS/$basearch/
enabled=1
gpgcheck=0
EOF
yum clean all
yum install subversion
svn --version
```
