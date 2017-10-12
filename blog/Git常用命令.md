<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-09-04
title: Git常用命令
tags: Git
images: 
category: Git
status: publish
summary: Git常用命令
-->

配置身份信息
```
git config --global user.name "name"
git config --global user.email "name@gmail.com"
```

创建本地仓库
```
//进入指定路径
git init
```

添加文件
```
//指定文件
git add fileName
//全部文件
git add .
```

提交

```
git commit -m "first commit"
```

忽略文件，在.gitignore文件中配置忽略文件
```
.idea
composer.lock
*.log
thinkphp
```

查看文件修改情况
```
git status
```

查看文件更改内容
```
git diff //所有文件
git diff 路径/file.java   // 指定文件
```

撤销
```
git checkout 路径/file.java   //(只适用于未add的文件)
git reset
```

查看提交记录
```
git log //查看全部
git log logId -1 //查看提交记录为 logId的1条
git log logld -1 -p//"-p"可以查看修改的具体内容
```

查看当前版本库的分支
```
git branch
```

创建分支
```
git branch newbranch     //创建分支newbranch
```

代码切换分支
```
git checkuot newbranch      //将代码切换到newbranch分支
```

合并分支
```
// 将newbranch合并到master
git checkout master
git merge newbranch
```

删除分支
```
git newbranch -D newbranch
```

clone
```
git clone http://github.com/example/demo.git
```

将本地仓库同步到远程仓库(push前请先确认已commit,否则无法同步)
```
//origin 是预先设置好的远程仓库地址
//master  是分支
git push origin master
```

将远程仓库同步到本地
```
git fetch origin master   //需要手动合并
git pull origin master    //自动合并
```

修改远程仓库路径
```
//直接修改
git remote origin set-url [url]

//先删除  后添加
git remote rm origin   
git remote add origin [url]
```
