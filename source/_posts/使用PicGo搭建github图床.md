---
title: 使用PicGo搭建github图床
date: 2019-07-05 15:30:54
tags: [picgo,github,images,hosting]
categories: web
---
![](https://raw.githubusercontent.com/cogito0823/photos/master/img/picgo.jpg)
<!--more-->
## 基本步骤
- 注册 GitHub 账号
- 创建一个 repository 作为图床
- 生成一个 personal token 用于连接PicGo
- 下载 PicGo 客户端
- PicGo的基本配置
- 设置 PicGo 快捷方式
- PicGo 的使用
## 注册 GitHub 账号
**GitHub地址：[http://www.github.com](http://www.github.com)**
## 创建一个 repository 作为图床
**（有疑问参考 [知乎：如何使用 GitHub？](https://www.zhihu.com/question/20070065?sort=created)，学习GitHub基本操作）**
## 生成一个 personal token 用于连接PicGo
**点击右上角用户头像在下拉菜单里点击 Settings 进入设置页面**

![](https://i.imgur.com/4qHa2e8.jpg)

**在设置页面左侧选择 Developer settings**

![](https://i.imgur.com/PkjtfIx.jpg)

**在左侧选择 Personal access tokens**

![](https://i.imgur.com/1DV6R5s.jpg)

**点击 Generate new token**

![](https://i.imgur.com/WF597Xq.jpg)

**在 Note 栏写上你的图床备注，如：For PicGo；选中下面的 repo,之后下拉到底部点击 Generate token**

![](https://i.imgur.com/nYJ9j41.jpg)

**保存生成的 token 序列**

## 下载 PicGo 客户端

**打开 PicGo 项目页面[https://github.com/Molunerfinn/PicGo](https://github.com/Molunerfinn/PicGo)**
**下载合适的客户端然后双击运行**
## PicGo 的基本配置

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/20190704170334.png)

- **仓库名：用户名/repositoryName**
- **分支名设为 master**
- **token 设为之前保存的 token 序列（如果之后使用时提示上传失败，则检查 GitHub 上是否存在你生成的 token，不存在则重新生成）**
- **存储路径指 GitHub 图床库内存放图片的路径，一般设为 img/**
- **自定义域名：https://raw.githubusercontent.com/用户名/图床名（即新建的 GitHub repository的名字）/分支名**

## 设置 PicGo 快捷方式
**选择 PicGo 设置，点击修改快捷键，设置为“ctrl + shift + c” （设置键位只需在键盘上敲出对应键位即可，不必打字输入）**

## PicGo 的使用

**一般操作系统在截图后会自动将图片放入剪贴板，PicGo 的快捷操作工作方式是敲入快捷键后将剪贴板的图片上传到图床（可选择上传前是否询问更改图片名称等属性，具体在 PicGo 设置中更改）**

When $a \ne 0$, there are two solutions to \(ax^2 + bx + c = 0\) and they are
$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
