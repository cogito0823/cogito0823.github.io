---
title: Tsunami系列（三）：Tsunami 学习
date: 
categories: software_testing
tags: [tsunami,安全,测试]
kewords: [tsunami,testing]
---
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/pexels-george-desipris-753619.jpg)
<!--more-->

## 概览
- 完成官方示例：检测 jupyter 漏洞
- 已知的插件种类
   - 侦察阶段
   - 漏洞检测阶段
- 谷歌目前给出的 Tsunami 插件
   - 端口扫描插件（Port Scanner）
   - 检测插件（Detectors）
- 总结


## 完成官方示例：检测 jupyter 漏洞

1. 安装依赖项

   ```
   nmap >= 7.80
   ncrack >= 0.7
   docker
   ```

   ```shell
   $ yum update
   $ yum install -y nmap
   $ yum install -y ncrack
   ```

2. 启动可以由Tsunami识别的易受攻击的应用程序，例如未经身份验证的Jupyter Notebook服务器。 最简单的方法是使用docker映像：

   ```shell
   $ docker run --name unauthenticated-jupyter-notebook -p 8888:8888 -d jupyter/base-notebook start-notebook.sh --NotebookApp.token=''
   ```

3. 执行以下命令：

   ```shell
   $ bash -c "$(curl -sfL https://raw.githubusercontent.com/google/tsunami-security-scanner/master/quick_start.sh)"
   ```

脚本 `quick_start.sh` 执行以下任务：

1. 把 [google/tsunami-security-scanner](https://github.com/google/tsunami-security-scanner) 和 [google/tsunami-security-scanner-plugins](https://github.com/google/tsunami-security-scanner-plugins) 仓库克隆到 `$HOME/tsunami/repos` 目录下。
2. 编译所有 [谷歌 Tsunami 插件](https://github.com/google/tsunami-security-scanner-plugins/tree/master/google) 并将所有插件的 `jar` 文件移动到 `$HOME/tsunami` 目录下。
3. 编译 Tsunami scanner Fat Jar 文件，并把它们移动到 `$HOME/tsunami` 目录下。
4. 移动 `tsunami.yaml` 示例配置到 `$HOME/tsunami` 目录下。
5. 打印示例Tsunami命令，使用先前生成的工具（artifacts）扫描 `127.0.0.1` 地址。

<img src="http://cos.ccogito.xyz/img/image-20200819142526013.png" alt="image-20200819142526013" style="zoom:200%;" />



## 已知的插件种类

### 侦察阶段

- `PortScanner` ：端口扫描器，用于扫描目标，识别出目标的端口、协议和服务。
- `ServiceFingerprinter` ：服务指纹识别器，根据端口扫描器得出的服务调用服务指纹识别器来识别出实际运行的应用，如 nginx 服务下运行 WordPress、php。

### 漏洞检测阶段

- `VulnDetector` ：漏洞检测器，根据侦查阶段提供的信息验证目标上的某些漏洞。

## 谷歌目前给出的 Tsunami 插件

### 端口扫描插件（Port Scanner）

- [Nmap Port Scanner](https://github.com/google/tsunami-security-scanner-plugins/tree/master/google/portscan/nmap)

### 检测插件（Detectors）

- [WordPress Exposed Installation Page Detector](https://github.com/google/tsunami-security-scanner-plugins/tree/master/google/detectors/exposedui/wordpress)
- [Exposed Hadoop Yarn ResourceManager API Detector](https://github.com/google/tsunami-security-scanner-plugins/tree/master/google/detectors/exposedui/hadoop/yarn)
- [Exposed Jupyter Notebook Detector](https://github.com/google/tsunami-security-scanner-plugins/tree/master/google/detectors/exposedui/jupyter)
- [Exposed Jenkins UI Detector](https://github.com/google/tsunami-security-scanner-plugins/tree/master/google/detectors/exposedui/jenkins)
- [Ncrack Weak Credential Detector](https://github.com/google/tsunami-security-scanner-plugins/tree/master/google/detectors/credentials/ncrack)

安装以上所有谷歌发行的插件：

```shell
$ ./build_all.sh
```

所有生成的 jar 文件都被复制到 `build/plugins` 目录

## 总结

- Tsunami 开源不久，github 也只给出一个简单的用例，开源的插件不多。
- 谷歌内部用 k8s 引擎做网段扫描，而开源的部分只可以扫描单个ip，可以通过编写 Tsunami 插件来扩展这个功能。
- github上给出了插件编写的方法。

- github上给出 Tsunami 的 dockerfile。
- 目前这个引擎和插件是java写的，谷歌说后续会添加其他语言编写的能力。

- Tsunami 集成了 nmap 和 ncrack 插件，它的扫描部分像是 nmap 的高级功能版本。

简单来说 Tsunami 的原理是先使用 nmap 进行端口扫描，识别出目标的端口、协议、服务，根据这些信息确定出目标在运行的应用，然后找出并运行这些应用对应的漏洞检测插件，最后得出漏洞检测报告。

由于 Tsunami 的端口扫描、应用识别和漏洞验证都可以自主选择插件进行，所以具有高度的扩展性（也可以说是把众多的安全检测工具自由地集成起来），比如端口扫描可以不必使用nmap（如果有更好的工具的话），可以使用更加全面的”指纹识别插件“来识别更多应用，添加更多的应用漏洞检测插件让漏洞检测覆盖的更加全面。

如谷歌所言，Tsunami 严重依赖插件系统来提供扫描功能。 Tsunami 在”侦察“阶段利用 nmap 等`PortScanner` 插件进行端口扫描来收集有价值的信息，在漏洞验证阶段又利用 `VulnDetector` 插件来针对特定的应用或服务进行漏洞检测。

目前谷歌发布的插件比较少，只有少数几个针对特定应用的检测插件，如wordpress、jupyter等。等插件开发得更多更全一些，Tsunami 就会变得更加成熟。 

1. 思考怎样让 Tsunami 扫描多个地址或者一个网段
2. 怎么把 Tsunami 以 docker container 的形式部署
3. 怎么分布式地使用 Tsunami 扫描多个地址
4. 学习编写 Tsunami 脚本
