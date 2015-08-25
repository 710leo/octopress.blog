---
layout: post
title: "用Octopress搭建博客"
date: 2014-09-14 13:01:14 +0800
comments: true
categories: 杂谈
---

博客以前一直放在BAE上，最近突然博客打不开了，发现原来BAE2.0的所有应用都下线了。。一直觉得Octopress不错，于是决定用Octopress+Github搭建一个博客，记录下搭建过程

我是在Ubuntu下安装的Octopress，参考的下面这篇文章   
[How to Install Octopress on Ubuntu Linux](https://www.jeremymorgan.com/tutorials/linux/how-to-install-octopress-on-ubuntu-linux-quantal-quetzal/)    
怎样将文章推送到github参考下面这篇文章   
[象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)

下面是过程中遇到的问题以及解决方法

问题1：
```
 ! [rejected]        master -> master (non-fast-forward)
```
解决办法： 用编辑器打开Rakefile文件，找到下面这行
```
system "git push origin #{deploy_branch}"
```
在#{deploy_branch}前面加(+)这个符号
```
system "git push origin +#{deploy_branch}"
```
运行下面命令
```
rake deploy
```
问题2：
```
/.rvm/gems/ruby-2.1.1/gems/execjs-2.2.0/lib/execjs/runtimes.rb:51:in `autodetect': Could not find a JavaScript runtime. See https://github.com/sstephenson/execjs for a list of available runtimes. (ExecJS::RuntimeUnavailable)
```
解决办法
```
sudo apt-get install nodejs
```
问题3:
```
An error occured while installing RedCloth (4.2.9), and Bundler cannot continue.
Make sure that `gem install RedCloth -v '4.2.9'` succeeds before bundling.
```
解决办法:
运行
```
gem install RedCloth -v '4.2.9
```
###Tips
- 提高页面载入的方法：将source/_include 文件下head.html 和footer.html中的关于google的链接删掉
- 最好在github创建一个分支将octopress整个目录push上去，这样当换电脑时，直接pull下来，配置文件什么的都不要修改