<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-06-01
title: Yii2.0 rules验证规则集合
tags: PHP,YII2
images: 
category: PHP
status: publish
summary: Yii2.0 rules验证规则集合
-->

required : 必须值验证属性
```
[['字段名'],required,'requiredValue'=>'必填值','message'=>'提示信息']; #说明:CRequiredValidator 的别名, 确保了特性不为空.
```

email : 邮箱验证
```
['email', 'email']; #说明:CEmailValidator的别名,确保了特性的值是一个有效的电邮地址.
```

match : 正则验证
```
[['字段名'],match,'pattern'=>'正则表达式','message'=>'提示信息'];

[['字段名'],match,'not'=>ture,'pattern'=>'正则表达式','message'=>'提示信息']; /*正则取反*/ #说明:CRegularExpressionValidator 的别名, 确保了特性匹配一个正则表达式. 
```

url : 网址
```
['website', 'url', 'defaultScheme' => 'http']; #说明:CUrlValidator 的别名, 确保了特性是一个有效的路径.
```

captcha : 验证码
```
['verificationCode', 'captcha']; #说明:CCaptchaValidator 的别名,确保了特性的值等于 CAPTCHA 显示出来的验证码. 
```

safe : 安全

```
['description', 'safe'];
```

compare : 比较
```
['age', 'compare', 'compareValue' => 30, 'operator' => '>=']; #说明:compareValue(比较常量值) - operator(比较操作符)  #说明:CCompareValidator 的别名,确保了特性的值等于另一个特性或常量.
```

default : 默认值
```
['age', 'default', 'value' => null]; #说明:CDefaultValueValidator 的别名, 为特性指派了一个默认值.
```

exist : 存在
```
['username', 'exist']; #说明:CExistValidator 的别名,确保属性值存在于指定的数据表字段中. 
```

file : 文件
```
['primaryImage', 'file', 'extensions' => ['png', 'jpg', 'gif'], 'maxSize' => 1024*1024*1024]; #说明:CFileValidator 的别名, 确保了特性包含了一个上传文件的名称.
```

filter : 滤镜
```
[['username', 'email'], 'filter', 'filter' => 'trim', 'skipOnArray' => true]; #说明:CFilterValidator 的别名, 使用一个filter转换属性.
```

in : 范围
```
['level', 'in', 'range' => [1, 2, 3]]; #说明:CRangeValidator 的别名,确保了特性出现在一个预订的值列表里.
```

unique : 唯一性
```
['username', 'unique'] #说明:CUniqueValidator 的别名,确保了特性在数据表字段中是唯一的.
```

integer : 整数
```
['age', 'integer'];
```

number : 数字
```
['salary', 'number'];
```

double : 双精度浮点型
```
['salary', 'double'];
```

date : 日期
```
[['from', 'to'], 'date','format' => 'yyyy-mm-dd'];
```

string : 字符串
```
['username', 'string', 'length' => [4, 24]];
```

boolean : 是否为一个布尔值
```
['字段名', 'boolean', 'trueValue' => true, 'falseValue' => false, 'strict' => true]; #说明:CBooleanValidator 的别名
```

image :是否为有效的图片文件
```
['primaryImage','image', 'extensions' => 'png, jpg,jpeg','minWidth' => 100,'maxWidth' => 1000,'minHeight' => 100,'maxHeight' => 1000,]
```
