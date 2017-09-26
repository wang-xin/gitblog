<!--
author: King
head: http://pingodata.qiniudn.com/jockchou-avatar.jpg
date: 2017-09-26
title: php 批量去空格和注释
tags: PHP
images: 
category: PHP
status: publish
summary: php 批量去空格和注释
-->

原理：php自带函数去注释和空格  => `php_strip_whitespace`
```
$base_dir = isset($_GET['dir']) ? $_GET['dir'] : ".";
check_dir($base_dir);

/**
 * 验证目录
 *
 * @param string $base_dir
 */
function check_dir($base_dir)
{
    if ($dh = opendir($base_dir)) {
        while (($file = readdir($dh)) !== false) {
            if ($file != '.' && $file != '..') {
                if (!is_dir($base_dir . "/" . $file)) {

                    $file_path = $base_dir . "/" . $file;
                    if (pathinfo($file_path, PATHINFO_EXTENSION) == 'php') {
                        file_put_contents($file_path, php_strip_whitespace($file_path));
                        echo "filename: $base_dir/$file " . " <br>";
                    }
                } else {
                    $new_base_dir = $base_dir . "/" . $file;
                    check_dir($new_base_dir);
                }
            }
        }
        closedir($dh);
    }
}
```
