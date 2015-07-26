---
layout: post
title: "用Octopress搭建博客"
date: 2014-09-14 13:01:14 +0800
comments: true
categories: 
---
<h4>前言</h4>
博客以前一直放在BAE上，最近突然博客打不开了，发现原来BAE2.0的所有应用都下线了。。一直觉得Octopress不错，于是决定用Octopress+Github搭建一个博客

<h4>介绍</h4>
Octorpress 其实是一个静态网站生成器，而gihub page提供托管静态页面的服务，用Octopress生产静态博客后，可以将静态页面放到github上，这样一个静态博客就诞生了
<h4>安装</h4>
我是在Ubuntu下安装的Octopress，参考的下面这篇文章    
[How to Install Octopress on Ubuntu Linux](https://www.jeremymorgan.com/tutorials/linux/how-to-install-octopress-on-ubuntu-linux-quantal-quetzal/)   
将octopress发布到github上   
[http://octopress.org/docs/deploying/github/](http://octopress.org/docs/deploying/github/)   

**一些Tip**   
 - 将source/_include 文件下head.html 和footer.html中的关于google的链接删掉或替换掉
 - 最好在github创建一个分支将octopress整个目录push上去，这样当换电脑时，直接pull下来，配置文件什么的都不要修改

