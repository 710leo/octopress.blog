---
layout: post
title: "任意文件遍历漏洞科普"
date: 2013-12-09 09:46:45 +0800
comments: true
categories: 
---
<h4>1 简介</h4>

文件遍历漏洞利用方法乍一看和文件包含差不多，其实有本质的差别，文件遍历的利用方法是通过路径直接访问资源， 当网站应用通过连接请求资源时，会根据实际完整路径查找资源，因此通过利用../等可以跨目录访问其他资源， 实例：

```
<?php
$img = $_GET['f'];
echo '<img src="images/'.$img.'">'
?>
```
访问a.com/openfile.php?f=a.jpg时,web应用会根据实际完整路径查找资源，如果a.jpg在/images 目录下，b.txt在 /config 下，则通过a.com/img?f=../config/b.txt 即可得到b.txt

<h4>2 利用所需条件</h4>

2.1 储存路径的变量没有任何过滤 
2.2 php.ini中open_basedir 设置不合理 
2.3 目录访问权限设置不合理

<h4>3 造成影响</h4>

2.1+2.2 可能造成系统的核心文件泄露 
2.1+2.3 可能造成应用程序的配置文件泄露

<h4>4 一些利用技巧</h4>

4.1 利用url编码绕过过滤

URL编码： 点–%2e 斜线–%2f 反斜线–%5c 
16位Unicode编码： 点–%u002e 斜线–%u2215 反斜线–%u2216 
双倍URL编码： 点–%252e 反斜杠–%u252f 正斜杠–%u255c

4.2 利用%00绕过文件后缀检测

例如：../../boot.ini%00.jpg，当获取文件名时.jpg被截断得到../../boot.ini

下面两种技巧没有测试

4.3 目录限定绕过:   
在有些Web应用程序是通过限定目录权限来分离的。当然这样的方法不值得可取的，攻击者可以通过某些特殊的符号“~“来绕过。 
形如这样的提 交“downfile.jsp?filename=~/../boot”。能过这样一个符号，就可以直接跳转到硬盘目录下了。

4.4绕过来路验证:    
在一些Web应用程序中，会有对提交参数的来路进行判断的方法，而绕过的方法可以尝试通过在网站留言或者交互的地方提交Url再点击或者直 接修改Http Referer即可，这主要是原因Http Referer是由客户端浏览器发送的，服务器是无法控制的，而将此变量当作一个值得信任源是错误的。

参考：
http://www.ruanyifeng.com/blog/2010/02/url_encoding.html 
http://www.nuanyue.com/archives/22.webcensor#postheada