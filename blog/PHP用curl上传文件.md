<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-03-06
title: PHP CURL文件上传
tags: PHP
images: 
category: PHP
status: publish
summary: PHP CURL文件上传
-->

```
<?php
//上传页面
$data = [
	'abc'=>123,    //普通参数
	'def'=>456,    //普通参数
	'pic'=>new CURLFile(realpath($file)),//$file是文件路径，(此处是php5.5以后的用法)，之前使用：'pic'=>"@$file"，@表示文件上传
];
$curl = curl_init();
curl_setopt_array($curl,[
	CURLOPT_URL =>'http://127.0.0.1/upload.php',    //图片上传地址
	CURLOPT_POST =>1,	
	CURLOPT_POSTFIELDS => $data,
	CURLOPT_RETURNTRANSFER=>true,
	CURLOPT_HEADER => 'content-type:application/x-www-form-urlencoded',  
        //CURLOPT_SAFE_UPLOAD=>false,
]);
$r = curl_exec($curl);
$errno = curl_errno($curl);
if($errno!=0){
	echo '请求出错'.$errno;
	echo curl_error($curl);
	die;
}
var_dump($r);

//接收页面
//直接能接收，像正常的接口一样。$_FILES
```
