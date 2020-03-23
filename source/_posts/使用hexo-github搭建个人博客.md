---
title: 使用hexo+github搭建个人博客
date: 2019-07-05 14:05:53
categories: web
tags: [hexo,github,blog]
kewords: [hexo,gitub,blog,hexo版本控制,nodejs,npm]
---
*平台：ubuntu18.04*
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/blog.jpg)
<!--more-->

## 步骤
- 准备
- Nodejs 的安装
- Hexo 的安装
- 部署到 GitHub
- 博客的版本控制

## 准备
在开始之前，你已经：
- **有一个github账号，账号配置了SSH Key**	
- **本机操作系统git客户端与GitHub有ssh连接**

## 安装Nodejs
去 [nodejs官网](https://nodejs.org/en/download/current/ "nodejs")  查看最新版本的nodejs，根据你的操作系统类型，在该页面选择合适的版本，并右键复制该版本的下载连接，如：https://nodejs.org/dist/v12.5.0/node-v12.5.0-linux-x64.tar.xz
目前最新：v12.5.0

```
$ wget https://nodejs.org/dist/v12.5.0/node-v12.5.0-linux-x64.tar.xz		#获取nodejs压缩文件到当前目录
$ tar xf node-v12.5.0-linux-x64.tar.xz	#解压缩到当前目录
$ cd node-v12.5.0-linux-x64/	#进入该目录
$ ./bin/node -v	#查看版本
```
创建 nodejs、npm 软链接
*注意创建软连接时使用绝对路径，软连接到/usr/local/bin/*

```
$ ln -s ~/node-v12.5.0-linux-x64/bin/node /usr/local/bin/
$ ln -s ~/node-v12.5.0-linux-x64/bin/npm /usr/local/bin/
```
如果你在创建软连接的时候，出现npm已经存在,node已经存在，则在删除 /usr/local/bin/目录下的node，npm后再创建。
```
$ sudo rm -rf /usr/local/bin/node
$ sudo rm -rf /usr/local/bin/npm

```
查看版本
```
$ node -v
v12.5.0
$ npm -v
6.9.0
```
## 安装HEXO
```
$ npm config set registry https://registry.npm.taobao.org	#国内主机需要更换淘宝源
$ sudo npm install -g hexo 	#安装hexo
```
初始化博客目录：
在电脑的某个地方新建一个名为blog的文件夹（名字可以随便取），比如：~/blog，这个文件夹作为将来存放博客代码的地方
```
$ mkdir blog	#新建blog
$ cd ~/blog	#进入blog
$ hexo init	#在 blog 文件夹下进行hexo初始化
```

```
$ hexo g	#生成静态页面
$ hexo s	#运行服务
```
顺利的话出现
```
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
此时打开本地浏览器输入 [http://localhost:4000/](http://localhost:4000/)看到hexo的helloworld开始页

![](https://i.imgur.com/PZW1DjG.jpg)

## GitHub 部署
添加一个hexo插件来deploy git
```
$ npm install hexo-deployer-git --save
```
修改 blog 目录下 _config.yml 文件，将deploy部分改写
```
$ vi _config.yml
```
找到
```
deploy:
  type:
```
改写成 （其中两个地方的 “username”改成你的github用户名）
```
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git
  branch: master
```
配置本机ssh以连接到 github
```
$ git config --global user.name "yourName"
$ git config --global user.eamil "email@example.com"
$ chmod 600 ~/.ssh/*	#改变.ssh文件夹下内容的权限
$ cd ~/.ssh/
$ mkdir id_rsa	#创建一个私钥文件
$ vi id_rsa	#在这个文件里写入GitHub的私钥
$ ssh-agent bash
$ ssh-add id_rsa	#应用这个私钥
$ ssh -T git@github.com	#测试能否连接 github
```
连接成功会看到
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

```
$ hexo d	#部署博客到github,即push
```

这时浏览器输入 [http://username.github.io](http://username.github.io)可以看到github上的hexo页面已经部署好了
## hexo 的版本控制
详见我的GitHub库 [cogito0823.github.io/README.md](https://github.com/cogito0823/cogito0823.github.io)
## 参考
- **[小茗同学的博客园>使用hexo+github搭建免费个人博客详细教程](https://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html)**

- **[飞翔的大马哈鱼>Ubuntu上用Hexo搭建博客托管到github](https://blog.csdn.net/lyb3b3b/article/details/78706077)**
