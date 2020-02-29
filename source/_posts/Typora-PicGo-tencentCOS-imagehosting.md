---
title: Typora-腾讯COS-PicGo-本地图片上传至图床
date: 
mathjax: false
tags: [Typora,PicGo,images]
categories: tools
---
Typora 新版本更新了上传本地图片到图床的功能，在编辑时可以把本地图片直接转为上传后的图片链接~😁
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/yellow-and-white-sail-boat-on-sea-3703074.jpg)
<!--more-->

## 基本步骤
- 新建腾讯COS对象存储
- 连接PicGo与腾讯COS
- 连接Typora与PicGo

## 新建腾讯COS对象存储
进入[腾讯云对象存储网站](https://cloud.tencent.com/product/cos)

注册一个腾讯云的账号，已经注册的话直接登录~

腾讯云有免费6个月的COS资源包，领取后创建一个存储桶

![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/创建存储桶.png)



![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/ChuangJianTong.jpg)

名称自定，访问权限选择共有读私有写

创建后新建几个文件夹进行分类，如`img`、`video`、`music`。

新建[云API密钥](https://console.qcloud.com/cam/capi)

![image-20200229220356859](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200229220356859.png)

## 连接PicGo与腾讯COS
打开PicGo进行图床设置---[PicGo下载页](https://github.com/Molunerfinn/PicGo/releases)

![image-20200229215750036](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200229215750036.png)

选择V5版本--Secretld、SecretKey、APPID参照云API密钥---存储空间名为存储桶的名字---存储区域参照“存储桶列表”中你的存储桶的“所属地域”如`ap-guangzhou`---如果你想把图片上传到你的bucket空间的某个文件夹下，则需要在PicGo里的指定存储路径里加上你的文件夹路径。比如`img/`（注意一定要加/）。

全部填好之后点确定并且要记得点击设为默认图床，这样上传才会默认走的是腾讯云COS。上传图片之后则会将图片自动上传到腾讯云COS里。

![image-20200229220030089](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200229220030089.png)

## 连接Typora与PicGo
下载/更新Typora---[Typora的页面](https://typora.io/)
在Typora 0.9.9.32（0.9.84）Beta更新中添加了上传功能，具体使用说明见[Upload Images](http://support.typora.io/Upload-Image/)

偏好设置-图像

![image-20200229222258062](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200229222258062.png)

上传服务选择PicGo(app)---PicGo路径选择PicGo程序所在位置

设置完后可以点击“验证图片上传选项”验证是否可用