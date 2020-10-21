---
title: Tsunami系列（二）：Tsunami Scan 是怎么工作的
date: 
categories: software_testing
tags: [tsunami,安全,测试]
kewords: [tsunami,testing]
---
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/pexels-george-desipris-753619.jpg)
<!--more-->

## 总览
**海啸安全扫描器（Tsunami security scanner）**的工作分为两个流程：
- **侦察（Reconnaissance）**: 第一步，Tsunami 首先识别出开放的端口，然后用一组“指纹识别”插件来识别目标主机上运行的协议，服务和其他软件。为了避免重复造轮子，Tsunami 利用了 nmap 等现有工具来完成其中一些任务。
- **漏洞验证（Vulnerability verification）**: 基于第一步收集的信息，Tsunami 运行所有与上一步识别出来的服务相匹配的漏洞验证插件，并给出无误报的漏洞验证结果。

## 总体工作流

Tsunami 的总体工作流如下图所示：



![image-20200818084534803](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200818084534803.png)



## 侦察（Reconnaissance）

在第一步的“侦察（Reconnaissance）”阶段，Tsunami探寻扫描目标并尽可能多的搜集它们的信息，包括:

*   开放端口，
*   协议，
*   网络服务和它们的 banners，
*   可能的软件和它们的版本。

Tsunami 的“侦察（Reconnaissance）”部分分两个阶段进行。

### 端口扫描阶段（Port Scanning Phase）

在端口扫描阶段，Tsunami执行端口扫描，以识别扫描目标上的开放端口，协议和网络服务。端口扫描阶段生成的结果是一个`PortScanReport` protobuf（Google Protocol Buffer，一种高效轻便的结构化数据存储方式，可以参考[深入 ProtoBuf - 简介](https://www.jianshu.com/p/a24c88c0526a)），它包含所有被端口扫描程序（PortScanner）识别出的`NetworkService`（网络服务）。

`PortScanner`是用于端口扫描目的的一种特殊类型的Tsunami插件设计。它使用户可以选择端口扫描的实现方式。为了避免重复造轮子，用户可以使用封装了[nmap](https://nmap.org/)或 [masscan](https://github.com/robertdavidgraham/masscan)等现有工具的 Tsunami 插件。你也许可以在 [tsunami-security-scanner-plugins](https://github.com/google/tsunami-security-scanner-plugins) 仓库中找到有用的`PortScanner`实现 。

### 指纹识别阶段（Fingerprinting Phase）

通常端口扫描程序只提供非常基础的检测能力。当扫描目标托管复杂的网络服务（例如Web服务器）时，扫描程序需要执行进一步的指纹识别工作，以了解暴露的网络服务的更多信息。

例如，扫描目标可能在同一个 TCP 443 端口上，使用 nginx 进行反向代理，运行了多个 web 应用，如 `/blog` 下的 WordPress， `/forum` 下的 php，等等。端口扫描程序只能识别出端口 443 正在运行 nginx 服务。 需要具有全面搜寻器（comprehensive crawler）的Web应用指纹识别器（Web Application Fingerprinter）来识别这些应用。

`服务指纹识别器（ServiceFingerprinter）`是 Tsunami 插件的一种特殊类型，允许用户为特定的网络服务定义指纹器。 通过使用过滤注释（filtering annotations，参阅 [如何将我的插件应用于某些类型的服务/软件？](https://github.com/google/tsunami-security-scanner/blob/master/docs/howto.md#filter_plugins)），Tsunami 能够在识别出网络服务时自动调用匹配的 `ServiceFingerprinters`。

### 侦察报告（Reconnaissance Report）

在侦查步骤结束时，Tsunami将端口扫描的结果和服务指纹识别的结果编译到单个 `ReconnaissanceReport` protobuf 中，以进行漏洞验证。

## 漏洞验证（Vulnerability Verification）

在漏洞验证步骤中，Tsunami并行运行 `VulnDetector` 插件，以根据侦察步骤中收集的信息验证扫描目标上的某些漏洞。 `VulnDetector` 的检测逻辑可以用纯Java代码实现，也可以以单独的二进制文件/脚本的形式，用其他语言（如python或go）实现。 外部二进制文件和脚本必须使用Tsunami的命令执行工具在Tsunami之外作为单独的进程执行。 有关使用其他语言编写 Tsunami 插件的设计想法，请参见 [Future Works](future_works.md#multi_lang_plugins)。

### 检测器选择 （Detector Selection）

通常，一个检测选择器 `VulnDetector` 仅验证一个漏洞，并且该漏洞通常仅影响一种类型的网络服务或软件。 为了减少重复工作，Tsunami 允许通过一些过滤注释（filtering annotation）对插件进行注释（有关详细信息，参见 [操作指南](https://github.com/google/tsunami-security-scanner/blob/master/docs/howto.md#filter_plugins)），以限制插件作用的范围。

然后，在“漏洞验证”步骤开始之前，Tsunami将基于扫描目标上暴露的网络服务和运行的软件，选择要运行的相匹配的 `VulnDetector`。 不匹配的 `VulnDetector` 将在整个扫描过程中保持未激活状态。
