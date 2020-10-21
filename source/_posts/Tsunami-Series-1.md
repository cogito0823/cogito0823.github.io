---
title: Tsunami系列（一） 介绍
date: 
categories: software_testing
tags: [tsunami,安全,测试]
kewords: [tsunami,testing]
---
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/pexels-emiliano-arano-1298684.jpg)
<!--more-->

## 概览
- 开发 Tusnami 的原因
- 目标和理念
- Tsunami Scan 是怎么工作的
- How do I ...
- Future Work

## 开发 Tsunami 的原因

当攻击者积极利用安全漏洞或错误配置时，组织需要快速做出反应，以保护潜在的易受攻击资产。 随着攻击者越来越多地投资于自动化，对新发布的高严重性漏洞做出反应的时间范围通常以小时为单位。 这对于拥有成千上万甚至数百万个互联网连接系统的大型组织，构成了巨大的挑战。 在这样的超大规模环境中，必须检测安全漏洞并以完全自动化的方式进行补救。 为此，信息安全团队需要有能力在非常短的时间内大规模部署和推出针对新型安全问题的检测器。 此外，重要的是检测质量始终很高。 为了解决这些挑战，我们创建了Tsunami-一种可扩展的网络扫描引擎，用于以无身份验证的方式高可信度地检测高严重性漏洞。

## 目标和理念

*   Tsunami supports small manually curated set of vulnerabilities
*   Tsunami detects high severity, RCE-like vulnerabilities, which often
    actively exploited in the wild
*   海啸产生的扫描结果具有很高的置信度和极低的假阳性率。
*   Tsunami detectors 易于实现。
*   Tsunami 易于扩展，执行速度快并且可以进行非侵入式扫描。

## Tsunami Scan 是怎么工作的

参见 [Tsunami Scan 是怎么工作的](https://confluence.tclking.com/display/test01/Tsunami+Scan+Orchestration#TsunamiScanOrchestration-Tsunami的扫描流程).

## How do I ...

*   ... [build and execute the scanner?](howto.md#build_n_execute)
*   ... [install Tsunami plugins?](howto.md#install_plugins)
*   ... [create a new Tsunami plugin?](howto.md#create_plugins)
*   ...
    [apply my plugins to certain types of services / software?](howto.md#filter_plugins)
*   ... [add command line arguments for my plugin?](howto.md#command_line)
*   ... [add configuration properties for my plugin?](howto.md#configuration)

## Future Work

See [Future Work](future_work.md).
