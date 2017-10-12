<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-10-12
title: 更改svn仓库地址
tags: svn
images: 
category: Linux
status: publish
summary: 更改svn仓库地址
-->

## 使用命令：
```
svn switch --relocate (Old Repository Root) (New Repository Root)
```

## 步骤：
##### 1. 进入工作复本
```
cd /data/app/nginx/html
```

##### 2. 查看仓库地址(URL)
```
svn info

Path: .
Working Copy Root Path: /data/app/nginx/html
URL: https://172.23.8.99/svn/niaoyun_api_domain/trunk
Relative URL: ^/trunk
Repository Root: https://172.23.8.99/svn/niaoyun_api_domain
Repository UUID: 60545dd1-9d56-464b-b6a6-878fab0a0b93
Revision: 148
Node Kind: directory
Schedule: normal
Last Changed Author: wangxin
Last Changed Rev: 148
Last Changed Date: 2017-10-12 14:29:06 +0800 (Thu, 12 Oct 2017)
```

##### 3. 更改仓库地址(URL)
```
svn switch --relocate https://172.23.8.99/svn/niaoyun_api_domain/trunk https://172.23.8.100/svn/niaoyun_api_domain/trunk

Error validating server certificate for 'https://172.23.8.99:443':
 - The certificate is not issued by a trusted authority. Use the
   fingerprint to validate the certificate manually!
 - The certificate hostname does not match.
Certificate information:
 - Hostname: cloud
 - Valid: from Sep 20 02:37:21 2017 GMT until Sep 18 02:37:21 2027 GMT
 - Issuer: cloud
 - Fingerprint: 89:20:7B:7C:6C:75:58:FB:43:BB:25:BD:4E:B4:F8:35:8C:DC:02:EB
(R)eject, accept (t)emporarily or accept (p)ermanently? 
```

##### 4. 更新文件
```
svn up
```
