---
title: Jmeter 的使用
date: 
mathjax: false
tags: [jmeter, 测试, 性能测试]
categories: 测试
---

![Jmeter](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/Jmeter.png)

Jmeter 接口测试的上手使用

<!--more-->

## 介绍

> Apache JMeter™ 应用程序是开源软件，这是一个 100％ 纯 Java 应用程序，旨在压力测试功能行为和性能测试。 它最初是为测试 Web 应用程序而设计的，但此后已扩展到其他测试功能。
>
> Apache JMeter可用于测试静态和动态资源、Web动态应用程序的性能。它可用于模拟服务器，服务器组，网络或对象上的重负载，以测试其强度或分析不同负载类型下的整体性能。

Jmeter 的特性包括：

- 能够负载和性能测试许多不同的应用程序/服务器/协议类型：
  - Web - HTTP, HTTPS (Java, NodeJS, PHP, ASP.NET, …)
  - SOAP / REST Webservices
  - FTP
  - Database via JDBC
  - LDAP
  - Message-oriented middleware (MOM) via JMS
  - Mail - SMTP(S), POP3(S) and IMAP(S)
  - Native commands or shell scripts
  - TCP
  - Java Objects
- 功能齐全的 Test IDE，允许快速记录测试计划（来自浏览器或本机应用程序）、构建和调试。
- CLI 模式（命令行模式（以前称为 Non GUI）/无头模式）可从任何 Java 兼容的操作系统（Linux，Windows，Mac OSX 等）加载测试
- 完整且随时可以呈现的动态 HTML 报告
- 通过从大多数流行的响应格式，HTML、JSON、XML 或任何文本格式中提取数据的能力，轻松实现关联
- 完全的可移植性和 100％ Java 纯度。
- 完整的多线程框架允许通过多个线程进行并发采样，并通过单独的线程组同时对不同的函数进行采样。
- 缓存和脱机 分析/重放 测试结果。
- 高度可扩展的核心
  - 可插拔采样器允许无限的测试功能。
  - 可脚本化的采样器（与 Groovy 和 BeanShell 等 JSR223 兼容的语言）
  - 可以使用可插入计时器选择几个负载统计信息。
  - 数据分析和可视化插件可实现出色的可扩展性和个性化。
  - 函数可用于为测试提供动态输入或提供数据处理。
  - 通过针对 Maven，Gradle 和 Jenkins 的第三方开源库轻松进行持续集成。

## 安装

先决条件：

- 安装 JDK 8
- 配置好 JAVA 环境变量
- win 10

### 下载

下载 Jmeter：[Jmeter Binaries](https://jmeter.apache.org/download_jmeter.cgi?Preferred=https%3A%2F%2Fdownloads.apache.org%2F) *（下载其中的 Binaries 就好，Source 包含了源码）*

![image-20201224174453256](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201224174453256.png)

win10 选择 第 2 条 zip 包（如果选择 tgz，可能需要解压两次）

### 解压

选择一个放置 Jmeter 程序文件夹的目录

将下载下来的 zip 或 tgz 包加压到该目录

我选择的目录：`D:\huahao2.chen\work\performance_test\environments\`

解压后的地址：`D:\huahao2.chen\work\performance_test\environments\Jmeter\apache-jmeter-5.4`

### 配置 Jmeter 环境变量

新增 `JMETER_HOME` 变量，注意：变量值为你下载后解压的路径。

![image-20201224175830254](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201224175830254.png)

编辑CLASSPATH变量，加上 `%JMETER_HOME%\lib\ext\ApacheJMeter_core.jar;%JMETER_HOME%\lib\jorphan.jar;`

![image-20201224180201782](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201224180201782.png)

## 使用

### 打开 Jmeter

点击 Jmeter 解压目录下的 bin 目录中的 `jmeter.bat`

会打开一个命令行窗口，稍等片刻会打开一个 Jmeter 的图形界面窗口，在不关闭命令行窗口的情况下，直接使用图形界面即可。

如图：

![image-20201224180817386](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201224180817386.png)

命令行中的提示：

```
================================================================================
Don't use GUI mode for load testing !, only for Test creation and Test debugging.
For load testing, use CLI Mode (was NON GUI):
   jmeter -n -t [jmx file] -l [results file] -e -o [Path to web report folder]
& increase Java Heap to meet your test requirements:
   Modify current env variable HEAP="-Xms1g -Xmx1g -XX:MaxMetaspaceSize=256m" in the jmeter batch file
Check : https://jmeter.apache.org/usermanual/best-practices.html
================================================================================
```

> 释义：“不要使用 GUI 界面进行压力测试！只在 GUI 界面进行压测的创建和调试。使用下面的命令来进行压力测试：`jmeter -n -t [jmx file] -l [results file] -e -o [Path to web report folder]`，并且修改 JMeter 批处理文件的环境变量：`HEAP="-Xms1g -Xmx1g -XX:MaxMetaspaceSize=256m"`”

**bin**目录中，还有一些其他脚本可能会有用。Windows 脚本文件（`.cmd` 文件需要 Win2000 或更高版本）：

- **jmeter.bat**

  运行JMeter（默认情况下处于 GUI 模式）

- **jmeterw.cmd**

  在没有Windows Shell控制台的情况下运行 JMeter（默认情况下处于 GUI 模式）

- **jmeter-n.cmd**

  在其上放置一个 JMX 文件以运行 CLI 模式测试

- **jmeter-nr.cmd**

  在其上放置一个 JMX 文件以远程运行 CLI 模式测试

- **jmeter-t.cmd**

  在其上放置一个 JMX 文件以在 GUI 模式下加载它

- **jmeter-server.bat**

  在 server 模式下启动 JMeter

- **mirror-server.cmd**

  在 CLI 模式下运行 JMeter Mirror Server

- **shutdown.cmd**

  运行 Shutdown 客户端以正常停止 CLI 模式实例

- **stoptest.cmd**

  运行 Shutdown 客户端以突然停止 CLI 模式实例

### 切换 GUI 语言为中文

Options -> Choose Language -> Chinese (Simplified)

![image-20201225155327804](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201225155327804.png)

### 从 Templates 新建测试计划

我们可以在已经存在的模板的基础上新建测试计划

具体的做法是：

点击菜单栏的 File -> Templates

这时候出现一个弹出窗口，然后可以从列表里选择一个模板：

![image-20201224182142810](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201224182142810.png)

## 创建测试计划

### 创建线程组

右键 Test Plan -> 添加 -> 线程（用户） -> 线程组

![image-20201225155820086](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201225155820086.png)

设置线程组和循环次数

![image-20201227131148001](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227131148001.png)

### 配置元件

右键线程组 -> 添加 -> 添加 -> 配置元件 -> HTTP 请求头默认值

![image-20201227131321362](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227131321362.png)

填入 Web 服务器的协议和 IP，填入 HTTP 请求的路径

![image-20201227131908950](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227131908950.png)

> 当所有的接口测试的访问域名和端口都一样时，可以使用该元件，一旦服务器地址变更，只需要修改请求默认值即可。

### 构造 HTTP 请求

右键 线程组 -> 添加 -> 取样器 -> HTTP 请求

![image-20201227132159443](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227132159443.png)

填入 Web 服务器和 HTTP 请求的值

![image-20201227132510724](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227132510724.png)

### 添加 HTTP 请求头

右键 线程组 -> 添加 -> 配置元件 -> HTTP 信息头管理器

（如果使用 POST 请求，用 Json 传值，那么需要设置请求头的 `ontent-Type:application/json`

![image-20201227133215388](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227133215388.png)

### 添加断言

右键线程组 -> 添加 -> 断言 -> 响应断言

根据响应的数据来判断请求是否正常。这里使用响应代码进行断言。

![image-20201227133629228](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227133629228.png)

![image-20201227133538167](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227133538167.png)

### 添加查看结果树

右键线程组 ->添加 -> 监视器 -> 查看结果树

添加后，点击运行，即可查看请求结果

![image-20201227134107149](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227134107149.png)

### 添加 Summary Report

右键线程组 -> 添加 -> 监听器 -> 汇总报告

添加后点击运行可以看到结果

![image-20201227134352657](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227134352657.png)

![image-20201227134442134](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227134442134.png)

### 保存测试计划

![image-20201227134543033](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227134543033.png)

## 执行测试计划

应该使用命令行来执行测试计划

进入 bin 目录

输入命令：

```
$ jmeter -n -t testplan/RedisLock.jmx -l testplan/result/result.txt -e -o testplan/webreport
```

来执行测试计划

### 输出的结果

**result.txt**

![image-20201227134838328](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227134838328.png)

**webreport**

![image-20201227134928189](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227134928189.png)



**Index.html**

![image-20201227134950261](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201227134950261.png)

## 参考链接

- [使用 JMeter 进行压力测试 - @晓晨Master](https://www.cnblogs.com/stulzq/p/8971531.html)
- [Jmeter 官方文档](https://jmeter.apache.org/usermanual/get-started.html)
