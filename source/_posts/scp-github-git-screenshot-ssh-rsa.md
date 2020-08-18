---
title: 传文件、改密钥对、连接GitHub、win10截图
date: 2019-11-05 03:20:47
tags: [linux,github,ssh,win10,scp,pscp]
mathjax: false
categories: linux
---
平时能用上的一些小技巧
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/sea-nature-beach-sand-21859.jpg)
<!--more-->
## 服务器间传文件

### linux间

用 `scp` 程序
![](http://q02jx5v1h.bkt.clouddn.com/picgo/20191104231437.png)

本地传远端：

```
scp -P 22 <file> <server>:dir
```

远端传本地：

```
scp -P 22 <server>:filedir dir
```

传输文件树，添加`-r`属性

如

```
scp -rP 22 <file> <server>:dir
```

### windows和linux间

用windows下的pscp程序

![](http://q02jx5v1h.bkt.clouddn.com/picgo/20191104232906.png)

用法基本同上



## 本地创建git仓库并同步到github

- 前提
  
- 使用过ssh和GitHub
  
- 本地新建文件夹作为仓库目录(也可以是想变成仓库的旧文件夹)

- 在该文件夹下输入命令进行git初始化

  ```
  git init
  ```

  如果该文件下原来有文件，输入命令将文件添加到仓库

  ```
  git add . #添加所有文件
  git add filename #添加特定文件
  git commit -m "自定义的提交信息"
  ```

   设置git的username和email，在github中每次提交都会记录他们 

  ```
  git config –global user.name “cogito0823” 
  git config –global user.email “cogito@net”
  ```

- 将本地文件`~/.ssh/id_rsa`里的内容复制到GitHub的新ssh密钥里

- GitHub上创建一个新的仓库，复制下该仓库的ssh链接，如`git@github.com:cogito0823/cogito0823.github.io.git`

- 在本地仓库根目录下输入命令将其和github上的新仓库连接起来

  ```
  git remote add origin <server>	#<server>替换为上一步复制的ssh链接
  git push -u origin master	#上传之前添加的文件
  ```

- 完成



## 自定义/自己选择linux的ssh密钥对

一般情况下系统使用用命令`ssh-keygen -t rsa`生成的密钥对`id_rsa`、`id_rsa.pub`。

如果自己之前有过一个密钥对，为了省事希望自己的所有密钥对都是同一个，可以通过改变上述密钥对文件来指定自己的密钥对。

- 找到自己要用的密钥对

- 备份旧的密钥对

  ```
  cd /root/.ssh
  mv id_rsa id_rsa.backup		#这里通过改名备份
  mv id_rsa.pub id_rsa.pub.backup  
  ```

- 把第一步找到的密钥对改名后覆盖旧的密钥对

  ```
  mv old_id_rsa id_rsa
  mv old_id_rsa.pub id_rsa.pub
  ```

- 把新的公钥放进文件 *authorized_keys* ，给新的私钥更新权限

  ```
  cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
  chmod 600 id_rsa	#没改权限在以后连接git时会报”Permissions xxx for 
			#'/root/.ssh/id_rsa' are too open.It is required that 
			#your private key files are NOT accessible by 	
			#others.This private key will be ignored.“的错误
  ```

-  重启ssh服务

  ```
  service sshd restart
  ```

- 完成



## win10下最快的截图方法

`ctrl`+`shift`+`s`
