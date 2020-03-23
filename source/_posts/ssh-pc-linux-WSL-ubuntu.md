---
title: 用ssh从外部连接到寝室个人电脑的Ubuntu系统或win10下的WSL--Linux子系统
date: 2019-10-27 18:35:47
mathjax: false
tags: [ubuntu,WSL,ssh]
categories: tips
---
当你懒得下床，电脑在床下的桌上，却想用手机来coding，或者你需要在实验室远程连接在寝室的电>脑的时候，你可以阅读这篇文章，试试把你的电脑武装成一台远程服务器~
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/blur-communication-computer-2148217.jpg)
<!--more-->
这篇文章我将以WSL（Windows下的linux子系统）为例演示一遍工作流程。
## 你需要提前了解的知识
- 知道啥是ssh、Linux
- 知道怎么用ssh连接服务器（不知道的话网上有很多教程可供学习）
- 会简单使用linux系统

## 步骤
- 开启WSL的ssh服务（linux系统也是）
- 设置防火墙以允许ssh端口的连接
- 找到你的公网ip(知道了这个ip才能在互联网上找到你的电脑)
- 用ssh连接你的电脑

## 开启WSL的ssh服务
进入WSL的命令行界面
键入命令`vi /etc/ssh/sshd_config`来修改ssh的配置文件
如果这个文件不存在就要安装openssh-server
```
apt-get install openssh-server
```
安装完后继续上一步修改配置。找到配置文件里的下列属性进行修改（记得把这些属性前面的#号删去，不然会被注释掉）
```
Port = 22 # 默认是22端口，如果和windows端口冲突或你想换成其他的否则不用动
#ListenAddress 0.0.0.0 # 如果需要指定监听的IP则去除最左侧的井号，并配置对应IP，默认即监听PC所有IP
PermitRootLogin yes # 如果你需要禁用 root 直接登录系统则此处改为 yes
PasswordAuthentication yes# 将 no 改为 yes 表示使用密码方式而非密钥登录
```
更改完上述属性后键入`service ssh start`启用ssh服务
如果提示`sshd error: could not load host key`则重新生成密钥，然后再次启动：
```
dpkg-reconfigure openssh-server
```
查看服务状态
```
service ssh status
# * sshd is running  显示此内容则表示启动正常
```
设置一下root用户的密码
```
passwd root
```
跟着提示设置好密码就进入到下一步设置防火墙。
## 设置防火墙以允许ssh端口的连接
打开Windows Defender 安全中心（可以在windows左下角搜索）进行如下设置：
![](https://raw.githubusercontent.com/cogito0823/photos/master/img/{892ABD49-7363-49CE-97E7-E3BABB7C0E00}.png)
![](https://raw.githubusercontent.com/cogito0823/photos/master/img/{31E6F21B-8894-464F-B659-784BF38D2EB9}.png)

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/sssss.jpg)

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/23456.jpg)

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/223232222.jpg)

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/w.jpg)

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/3323ds.jpg)

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/image-20191027181607110.png)

名称和描述自己设置
## 找到你的公网ip(知道了这个ip才能在互联网上找到你的电脑)
博主在读学校分配了公网ip，因此只需要上网查自己的ip就能得到自己的公网ip。
如果你也有公网ip那么打开https://link.zhihu.com/?target=http%3A//www.net.cn/static/customercare/yourip.asp 、http://www.ip138.com 两者中的一个网站可以查看你的ip。
## 用ssh连接你的电脑
有了公网IP就可以通过ssh在局域网连接你的电脑了
```
ssh root@hostname
```
如果你在lunux系统上操作就键入上面的命令（把上面命令里的hostname改成你的ip），然后会询问是否接受这次连接，选接受后会提示你输入root用户的密码，输入以后就连接成功了。
其他ssh工具只要确定连接方式是ssh、端口是你设置的数字（默认22）、ip是你的公网ip就可以。
这样就完成了远程连接个人电脑的操作，如果你不是wsl系统用户，那么防火墙设置只需要按照网上的开启ssh服务里的防火墙相关设置进行就可以了。上网搜索`Linux ssh防火墙设置`就会有很多可用的结果。
