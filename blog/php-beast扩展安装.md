<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-07-05
title: php-beast 扩展安装使用
tags: php,linux
images: 
category: PHP
status: publish
summary: php-beast 扩展安装使用
-->

### 一、下载、编译 ###
```
 wget https://github.com/liexusong/php-beast/archive/master.zip
 unzip master.zip
 cd php-beast-master
 phpize
 ./configure
 make && make install
```
编译好之后修改php.ini配置文件, 加入配置项, 重启php-fpm
```
 extension = beast.so
 beast.enable = On
 beast.cache_size = size			#设置缓存大小 默认10M
 beast.log_file = "path_to_log"
 beast.log_user = "user"
 beast.log_level = "debug"
```

### 二、项目加密 ###
1. 加密项目
安装完 php-beast 后可以使用 tools 目录下的 encode_files.php 来加密项目，使用 encode_files.php 之前先修改 tools 目录下的 configure.ini 文件，如下：
	```
	 ; source path
	 src_path = ""
	
	 ; destination path
	 dst_path = ""
	
	 ; expire time
	 expire = ""
	
	 ; encrypt type (selection: DES, AES, BASE64)	加密方式
	 encrypt_type = "DES"
	```
 	`src_path` 是要加密项目的路径，`dst_path` 是保存加密后项目的路径，`expire` 是设置项目可使用的时间 (expire 的格式是：YYYY-mm-dd HH:ii:ss)。`encrypt_type` 是加密的方式，选择项有：DES、AES、BASE64。 修改完 configure.ini 文件后就可以使用命令`php encode_files.php` 开始加密项目。


2. 加密文件
安装完 php-beast 后可以使用 tools 目录下的 encode_file.php 来加密单个文件，如下：
	```
	php encode_file.php --oldfile old_file_path --newfile new_file_path --encrypt DES --expire "2020-12-31 23:59:59"
	```

### 三、安全 ###
下载解压后：
1. 修改 header.c 加密后的文件头结构，增加加密的安全性。
	```
	char encrypt_file_header_sign[] = {
		0xe8, 0x16, 0xa4, 0x0c,
		0xf2, 0xb2, 0x60, 0xee
	};
	```

2. 在指定的机器上运行。要使用此功能可以在 networkcards.c 文件添加能够运行机器的网卡号，例如：
	```
	char *allow_networkcards[] = {
		"fa:16:3e:08:88:01",
	    NULL,
	};
	```
	设置之后，扩展就只能在 `fa:16:3e:08:88:01` 这台机器上运行。另外要注意的是，由于有些机器网卡名可能不一样，所以如果机器的网卡名不是 `eth0` 的话，可以在 `php.ini` 中添加配置项： `beast.networkcard = "xxx"` 其中 `xxx` 就是你的网卡名，也可以配置多张网卡，如：`beast.networkcard = "eth0,eth1,eth2"`。

3. 使用该扩展时不要使用默认的加密key，因为扩展是开源的，如果使用默认加密`key`的话，很容易被人发现。所以编译的时候修改加密的`key`，`aes模块` 可以在 `aes_algo_handler.c` 文件修改，而 `des模块` 可以在 `des_algo_handler.c` 文件修改。
