---
title: Java 云端白盒测试专项调研
date: 
mathjax: false
tags: [测试]
categories: 测试

---

![java](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/java.png)

<!--more-->

## 概要

- 背景
- 思路
- 案例分析
  - 一个静态代码扫描实现
  - 酷家乐代码度量平台开发实践
  - 优酷播放白盒测试体系
- 推荐实现
  - 工具栈
  - 实现的功能
  - 工作流
  - 交互参考

## 背景

随着产品业务发展、人员的流入，产品迭代速度的提升，开发团队与测试团队之间的合作出现新的技术挑战。

1. 开发过程中不能对代码本身的质量情况得出合适的反馈，后期中大型测试出现错误和漏洞的概率较大，查找和消除错误的成本较高，项目部署风险较大。
2. 随着业务迭代越来越多,代码行数越来越多.可能会引入重复代码、代码复杂度高、不符合编程规范以及安全漏洞等问题。代码合并过程中容易出现冲突。
3. 人员流动带来编码风格、编码规范的多样，使代码可读性得不到保障。
4. 累积的单元测试数据不能很好的显现，测试覆盖率没有数据支撑。
5. CI/CD 的提倡。

这个时候需要通过一些技术手段来解决代码规范检查的问题，协助开发完成单测、安全检查。通过一些自动化的方式来度量开发的代码质量，并能够及时发现问题，保证开发提交的代码都是高可用，高质量的。同时可以提供一些有效的量化结果，指导做一些流程和技术上的优化。保证代码的高质量，高可用性，高可读性，随时保证代码分支的代码纯净，并能够很好的支持 CI/CD。

## 思路

结合现行 CI/CD 方案，整合合适的白盒测试工具，开发出一套云端白盒测试框架，实现以下功能：

- 持续集成
- 代码规范检查
- 代码审计
- 单元测试覆盖率
- 代码质量度量。

构建一条自动化流水线,从提交代码 => 扫描代码 => 发送报告，发现违背程序编写标准的问题，程序中不安全、不明确和模糊的部分，找出程序中不可移植部分、违背程序编程风格的问题，包括变量检查、命名和类型审查、程序逻辑审查、程序语法检查和程序结构检查等内容，以满足背景中提及的需求。

白盒测试按照是否执行代码分为静态测试和动态测试，静态测试有 **code review** 和 **静态代码扫描** 两种方式。

**静态代码扫描** 可以实现：

- 代码规范检查
- 代码审计
- 三方包漏洞扫描

因此，结合静态代码扫描 + 单元测试框架 + 测试覆盖率模块 + 持续集成工具 + 前端展示，可以构建期望中的云端白盒测试框架。

### 静态代码扫描

静态代码扫描的价值：

1. 研发过程，发现BUG越晚，修复的成本越大；
2. 缺陷引入的大部分是在编码阶段，但发现的更多是在单元测试、集成测试、功能测试阶段；
3. 在整个软件开发生命周期中，30% 至 70% 的代码逻辑设计和编码缺陷可以通过静态代码分析来发现和修复的。

优点：

- 不需要启动服务,就能扫描代码；
- 通过规则自动化扫描代码；
- 支持多语言；
- 人力成本低。

缺点：

- 不能发现代码业务逻辑问题（需要通过 code reviews 实现）；
- 可能会扫描出很多问题，影响开发进度。

**Code Review** 可以作为静态代码扫描的补充，找出代码业务逻辑问题，具体有：

- 数据引用
- 数据类型
- 数据声明
  - 初始化
  - 变量名
- 计算错误
- 逻辑运算
- 控制流程
- 子程序参数错误
- 输入输出错误
- 其他错误

#### **静态代码扫描工具/引擎**

| 序号 | 引擎       | 分析对象   | 特点                                                         |
| ---- | ---------- | ---------- | ------------------------------------------------------------ |
| 1    | Findbugs   | 字节码     | 缺陷模式匹配、数据流分析。通过字节码分析代码存在缺陷、支持数据流分析，能查找出空指针崩溃等问题。 |
| 2    | CheckStyle | Java源文件 | 缺陷模式匹配。扫描代码格式规范、代码风格的工具。             |
| 3    | Godeyes    | ～         | 百度出品，主要针对Android代码。                              |
| 4    | Lint       | Java源代码 | Android官方提供的代码分析工具，可以扫描出API兼容性问题、布局性能等针对Android代码的潜在缺陷。 |
| 5    | PMD        | Java源代码 | 缺陷模式匹配、数据流分析。检测代码潜在错误。                 |

#### SonarQube 平台

单独使用静态代码扫描工具时，我们会面临：

1. 需要一个平台，能够汇总呈现不同语言的项目、不同工具的扫描结果，
2. 需要一个平台，可以友好的呈现或者追溯，一段时间内每一次扫描的结果的差异。

使用 SonarQube 可以以插件或规则的形式整合其他工具来解决这些问题。

##### 什么是 [SonarQube](https://docs.sonarqube.org/latest/) ：

- 是一个代码质量和安全分析平台。
- 可以多维度分析代码：代码量、安全隐患、编写规范隐患、重复度、复杂度、代码增量、测试覆盖率等。
- 支持27+编程语言的代码扫描和分析，包含java\python\C#\javascript\go\C++等。
- 涵盖了编程语言的静态扫描规则： 代码编写规范+安全规范。
- 能够与 IDE、CI/CD 平台集成。
- 能够与 SCM 集成，可以直接在平台上看到问题代码是由哪位开发人员提交。
- 帮助开发写出更干净、更安全的代码。

##### SonarQube如何工作：

Sonar静态代码扫描由 2 部分组成：SonarQube 平台，sonar-scanner 扫描器。

SonarQube: web 界面管理平台。

- 展示所有的项目代码的质量数据。
- 配置质量规则、管理项目、配置通知、配置SCM等。

SonarScanner: 代码扫描工具。

- 专门用来扫描和分析项目代码。支持27+语言。

- 代码扫描和分析完成之后，会将扫描结果存储到数据库当中，在SonarQube 平台可以看到扫描数据。

SonarQube平台中支持整合各种静态代码扫描检测工具。SonarQube中各种代码检测工具分析对象及应用技术对比：

| Java静态分析工具 | 分析对象   | 应用技术                 |
| ---------------- | ---------- | ------------------------ |
| CheckStyle       | Java源文件 | 缺陷模式匹配             |
| FindBugs         | 字节码     | 缺陷模式匹配；数据流分析 |
| PMD              | Java源代码 | 缺陷模式匹配             |

##### 与gitlab 集成

![使用SonarQube从GitLab发出的拉取请求](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/gitlab-gq-failed.png)



## 案例分析

本文调研了[一个静态代码扫描实现](https://testerhome.com/topics/24261)、[酷家乐代码度量平台开发实践](https://testerhome.com/articles/24210)、[优酷构建覆盖全网的播放白盒测试体系](https://www.infoq.cn/article/w9MxsRSEZWlkJFeojhEz) 三种实现方案。

### 一个静态代码扫描实现

#### 工具栈：

gitlab + jenkins + SonarQube

> 除了Jenkins，还可以尝试gitlab ci，这种方式接入成本更低，体验更好。有插件可以直接把问题展示在commit页面，对研发的体验更好。

#### 结果验证：

- 证明静态代码扫描是有可行性的、发现扫描项目不少于5个严重问题
- 可以形成 bug 统计报告

#### SonarQube 扫描规则

可以是阿里P3C规则，或者控制扫描等级等。

>  规则集的选择不仅要考虑问题等级，还要考虑业务特性，比如有些业务比较在意安全性，可以把安全相关的规则都加上。

#### 扫描分支

可能一个项目会被扫描多个分支,需要在SonarQube区分代码分支（不区分分支会导致代码检查时大量报命名相似的错误）。

> 多分支支持是sonarqube的收费版本的特性之一，但是社区也有开源插件支持，比用多项目区分效果好一些。

以maven项目为例子:

```
mvn sonar:sonar -Dsonar.branch=xxxx
```

SonarQube 对应的项目名字后缀的分支名字

[![image](http://note.youdao.com/yws/public/resource/b57c4b4962c0fe36ff75ac041815b271/A43DBC214B314A509EC939E2D479A46A?ynotemdtimestamp=1592142693998)](http://note.youdao.com/yws/public/resource/b57c4b4962c0fe36ff75ac041815b271/A43DBC214B314A509EC939E2D479A46A?ynotemdtimestamp=1592142693998)



#### 自定义报告 

SonarQube 不能查看历史数据,需要每次扫描完成后,把扫描结果存一份到测试管理平台上。步骤：

- **SonarQube 中添加 webhook**

  ![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/82BD0842A2ED4565BE7CEA78FEFAF0F1)

  

- **保存报告后端接口**

  会用到 SonarQube 的一些 api 接口：

  ![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/CC5804FC4CB3472A846401E09B90EC69)

  

- **前端展示报告**

  ![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/C8C80FC47BE845CA831E20039C12C80B)



#### 测试通知

每次静态代码扫描完成后都会把结果发到企业微信群中。

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/FCB78FCDE6994EE29E934ACF22CE6B1A)



#### **扫描策略**

- 监控活跃或者master分支,当提交代码的时候触发自动化代码扫描。
- 部署服务后触发自动化代码扫描。
- 每天晚上定时自动化代码扫描。
- 上线打包构建自动化代码扫描。



> 静态代码扫描是相当长的测试周期、并且覆盖项目的范围比较广。
>
> 如果一个公司像尝试去做这方面,最好有一个架构涉及或者清晰的调研结果。
>
> 和自动化构建工具结合的时候,尽量使用开源的、或者公司内部的自动部署平台。
>
> 在人员投入上,最好能有一个专职的人去做,并且有一定的代码基础。



### 酷家乐代码度量平台开发实践

#### 工具栈

- 静态代码扫描（代码度量工具链）
  - SonarQube
    - 编码规约 - P3C规则
  - Cobra（代码审计）
  - Synk（三方包漏洞扫描）
- SonarLint（集成到IDE）
- gitlab
- gitlab CI

#### 技术架构

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/8980b234-f972-4534-a6d8-7029bfd121a7.!large)

#### 统一度量

质量总分 = 单测通过率得分 + 单测行覆盖率得分 + 重复度得分 + 复杂度质量分 + 坏味道质量分 + BUG质量分 + 漏洞质量分

•单测通过率 得分 = 单测通过率 * 权重，权重 5分
•单测行覆盖率 得分 = 单测行覆盖率 * 权重，权重 5分
•重复度 得分 = 10 - 重复度，权重 5分（<=5% , 满分, >=10% , 0分，每增加 1% 扣 20%)）
•复杂度质量分 = 5 * (1 - 千行复杂度/千行复杂度质量红线 ) ，权重 5分 （>质量红线，0分）
•坏味道质量分 =7 * (1 - 千行坏味道阻断数/千行坏味道阻断数质量红线 ) + 2 * (1 - 千行坏味道严重数/千行坏味道严重数质量红线 ) + 1 * (1 - 千行坏味道主要数/千行坏味道主要数质量红线 ) ，权重 10分 （>质量红线，0分）
•BUG质量分 =20 * (1 - 千行BUG阻断数/千行BUG阻断数质量红线 ) + 7 * (1 - 千行BUG严重数/千行BUG严重数质量红线 ) + 3 * (1 - 千行BUG主要数/千行BUG主要数质量红线 ) ，权重 30分（>质量红线，0分)
•漏洞质量分 =28 * (1 - 千行漏洞阻断数/千行漏洞阻断数质量红线 ) + 8 * (1 - 千行漏洞严重数/千行漏洞严重数质量红线 ) + 4 * (1 - 千行漏洞主要数/千行漏洞主要数质量红线 ) ，权重 40分（>质量红线，0分）

通过调整权重快速改变质量分的分值，以达到代码质量持续改进的目标。

#### 平台运营

数据驱动+流程卡点驱动代码质量提升

##### 数据驱动

代码质量趋势图，以业务组维度，度量研发周期内的代码质量趋势。

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/a6eae4d9-4d48-441d-8ae7-cca23a591687.png!large)

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/d8f86236-66b5-488e-bcdc-b201361b4800.png!large)



单测Top红黑榜排名，通过代码质量通晒的方式来督促研发质量提升

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/929460ad-6006-421e-8abb-b7ffa7ca81d0.png!large)



##### 流程卡点

结合发布系统，进行部署节点的质量卡点

各业务线可自定义应用质量红线，形成业务线质量公约。

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/c85f19ac-ee56-495e-b224-a851b9498537.png!large)



应用每次CD进行质量检测，显示质量分析报告，质量报告包含(接口测试，代码扫描，二方包检查，sql预检)四部分内容，全部指标达标才可发布。

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/e401a437-c192-4995-868c-e3759c3b28a9.png!large)



超过阈值的应用质量将显示不达标状态，质量检测不达标的应用限制流转，如需紧急发布，需申请上级Owner审批。

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/fd29dd7b-4c0f-40db-85af-af91da96df46.png!large)



正常流转

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/48965458-e38b-444f-9720-84a8c98f2c6c.png!large)



#### 质量运营

> ###### 阶段1: 代码违规清理
>
> 代码度量初始阶段，我们的工作重点是解决代码中可能导致Bug的隐藏问题，规范代码编写。这个阶段我们度量重点指标为 阻塞/致命/严重 级别的 Bug 和 Vulnerability 个数，映射到质量分中的数据为 BUG质量分，漏洞质量分，所以这个阶段的这两个部分的权重分别为30，40，即若出现 2 个 以上 阻塞/致命/严重的问题，该仓库的质量分将达不到 质量红线 65.
>
> ###### 阶段2: 重复代码治理
>
> 在经历阶段1 的 问题清扫之后，代码中的明显潜在问题都已经消除，我们转向代码治理方向。
> 每个程序员的极致追求，都是写出一份优雅的代码，那什么是好的代码？
>
> 这阶段的重点是将代码从"ctrl+c"到"ctrl+v"解放出来，从代码能工作到代码整洁开始演化，这个阶段我们度量重点指标为 代码重复度，业界关于重复度的指标是10%, 映射到质量分中的数据为 重复度质量分，即重复度超过10%，质量分为0，借助质量卡点 ，要求重复质量分小于1分的项目不允许流转到下一开发阶段。
>
> ###### 阶段3: 质量内建
>
> 代码治理之后是质量内建，该阶段的目标 开发提交的存量功能都是有单测保证的，增量功能都是经过单测覆盖的。这个阶段我们度量重点指标为 单测通过率和单测覆盖率，因此我们调整质量权重， 单测通过率 权重 5增加20，阈值100%，单测行覆盖率 5增加15分，阈值30%。
>
> ###### 阶段4: 设计质量
>
> 这个是综合比较的阶段，我们引入了 安全风险扫描，代码审计，开源协议扫描等新的检测手段，再结合单元测试通过率、单元测试覆盖率、静态代码质量等原始指标，综合的评估一个项目代码的设计质量，是在固化前3阶段的成果之上，对代码质量的一个更高追求，我们也正在为之奋斗！

#### 愿景

> 代码门禁：守卫进入主分支的每一个commit
>
> ##### 结合Git Flow，将CD流程的质量卡点引入Gitlab CI流程:
>
> 1、每次代码提交触发代码静态扫描
> 2、每次合并各发布分支前检查代码质量是否符合质量标准
>
> ##### 质量左移: 测试从第一行编码开始
>
> 1、SonarLint 插件，实现在代码编码过程中的实时扫描，将问题暴露在编码阶段
> 2、结合 Gitlab Webhook 功能，当前代码出现致命，阻塞级别的问题时，不允许代码提交
>
> ##### 编码BP: 你的每一行都是编码最佳实践
>
> 1、收集 Code Review 最佳编码实践，形成编码知识库
> 2、在研发内部分享最佳编码实践
> 3、结合 Idea 插件在编码阶段自动提示最佳实践

#### 遇到的问题

问题一 怎么解决这个成本和收益的问题呢？

> 在平台落地过程中, 我们也发现单测的成本问题, 不仅是研发侧为提高单测覆盖率付出的实际编码成本, 更是研发侧对覆盖率指标的质疑。
>
> 我们摸索出的可行模式: 收窄覆盖率指标, 看的见的收益
>
> 1. 从整个Repo的覆盖率指标收窄为热点代码块的覆盖率指标, 热点代码块的来源是根据线上代码执行的频率选取的
> 2. 从历史代码问题导致的Bug中, 统计出能在单测阶段发现的问题, 并以此形成单测改进规范, 推动规范落地 这样达到的目的就是让研发侧真切的感知到, 你的单测覆盖是真正跟线上功能和故障捆绑在一起的, 而不是一个KPI指标。

问题二 单测的覆盖率目标是怎么做到合理的设定？

> 从以下几个维度考量设定：
>
> 1. 当前项目单测实际情况
> 2. 当前项目的是否为核心服务
> 3. 当前项目本季度的业务开发繁忙情况
> 4. 覆盖率指标选取(行/方法/分支...)
>
> 总结下来就是: 在不影响业务开发情况下的一个够得着的覆盖率指标。

### 优酷播放白盒测试体系

播放器作为优酷的核心基础模块，为点播、直播、短视频等业务提供基础播放能力，播放质量保障过程涉及到众多业务方和大量回归适配工作，因此迫切需要自动化能力。优酷团队经过一系列调研，认为传统的 UI 自动化方案在播放场景下存在一系列弊端：

- 无法有效覆盖各类播放策略的正确性和有效性；
- 依赖相对固化的 UI 元素层级，与优酷快速迭代的节奏不 match；
- case 开发维护成本高，投入产出低；
- 播放场景强依赖网络环境，UI 自动化在可控网络方面不够灵活。

为解决以上问题，优酷测试团队自研出一套基于 AFrame 的白盒自动化测试方案，并在此基础上构建出基于实验室环境的播测验证体系和基于外网环境的动态拨测体系。

#### 框架结构

基于 AFrame 的白盒自动化测试框架由 5 个核心模块组成：AFrameSDK、AFrameServer、CaseClient、PKAT 和 AFrameService。

其中 AFrameSDK 作为移动端侧模块，集成到优酷 app，其余 4 个模块均可脱离移动端，支持单独部署，各模块通过 WebSocket 或者 Http 进行交互，整体交互关系如下：

![s](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/c425e480eaf01190386a49c05404071a.png)

1. AFrameSDK：自动化测试的移动端入口模块，该模块包含与 AFrameServer 长连的 WebSocket Client，用于接收和解析 AFrameServer 下发的测试指令、调度执行、过程同步、数据收集和转发；
2. CaseClient：通过改造 JUnit 测试框架为每个用例创建一个 WebSocket Client，向 AFrameServer 发送执行指令，并接收其中转的测试结果，完成行为分析和数据判断；
3. AFrameServer：框架的数据“中转站”，监听来自 CaseClient 和 AFrameSDK 的连接请求，为 CaseClient 和 AFrameSDK 建立一对一的连接通道；
4. AFrameService：用于用例管理、执行管理、设备管理、结果管理、配置和任务下发等；
5. PKAT：播放测试结果校验和分析中心，是播放业务的“问诊台”，多端播放测试用例执行后，由其根据实际场景对 Log、VPM 埋点、接口数据等关键信息进行分析校验。

##### 移动端自动化调度员：AFrameSDK

作为整个测试框架的入口模块，也是 5 个模块中唯一嵌入移动端的模块，AFrameSDK 承担着接口自动化调度员的角色。AFrameSDK 的主要工作流程如下：

1. 与 AFrameServer 建立连接，等待 AFrameServer 下发执行指令；
2. 接收执行指令、解析指令、驱动执行；
3. 执行数据反馈，并发送回 AFrameServer。

AFrameSDK 接收到 AFrameServer 下发的指令后，通过对应的规则完成对指令的解析并驱动 app 执行。AFrameSDK 内置了若干通用的基础工具库供业务方使用，业务方也可以根据自身测试需求，定制化基于业务的测试驱动接口，目前已接入并具备定制化场景测试能力的业务方包括播放器、缓存、来疯等。

![ss34t4fds](https://static001.infoq.cn/resource/image/a3/26/a358a236ccd8e342575c9af766504926.png)

##### 数据中转服务：AFrameServer

指令和数据是自动化测试的核心物料，AFrameServer 作为指令下发和数据传输的中心节点，承担着“搬运工”的角色，AFrameServer 将接其收到的连接请求分成如下几类：

1. 移动设备端连接；
2. 用例端连接；
3. AFrameService 连接，用于设备同步、任务同步等；
4. 功能扩展连接，如内外网穿透需求等。

AFrameServer 接收到上述连接请求后，根据分类和连接标识管理所有连接，当连接请求 A 需要和连接请求 B 进行通信时，需指定 B 的连接标识信息，AFrameServer 通过该连接标识为 A、B 提供数据转发服务。

![w4](https://static001.infoq.cn/resource/image/c9/08/c92cc9d06ee2da64376389fb4d660008.png)

##### 基于 JUnit 的用例设计：CaseClient

用例设计是最重要的测试环节之一，用例设计的好坏和覆盖度直接决定了测试效果，为了提高用例设计和开发效率，对 JUnit 框架进行二次改造，封装改造后的用例设计 CaseClient 主要具备以下优势：

1. 支持多用例实例执行隔离，尤其当用例与移动设备存在一对一关联关系时，可做到互不干扰；
2. 支持根据执行过程动态控制用例执行，如播放失败后处理、Crash 后优雅终止；
3. 支持可控化参数输入，如用于打通限速服务的可控网络输入、测试数据输入、执行参数控制等；
4. 面向接口编程，无需要关注与 AFrameServer 间的交互逻辑。

##### 结果管理与任务下发：AFrameService

AFrameService 作为测试启动入口，承担着“大管家”的角色，该模块提供了一系列接口供用户使用或测试平台调用，核心工作流程如下：

1. 接收 AFrameServer 同步端设备信息；
2. 向 CaseClient 发起测试用例任务执行命令；
3. 执行完成后，CaseClient 将执行基础数据、执行过程数据回传至该模块，同时异步调用 PKAT 对结果进行分析和校验；
4. PKAT 分析完成后将结果上传至该模块存档。

#### 基于实验室环境的播测能力

基于实验室环境的播测能力主要包括，播放核心 topic 自动化 & 常态化测试能力和播放器基础质量评估能力（包含：线下稳定性评估、基础性能评估、VPM 基础校验）。下面以智能档为例，简要介绍播测系统在实际业务中的落地实践情况：

1. 常规测试方法：播放上层业务可以通过播放控制面板的 UI 交互获得视觉上的反馈，智能档区别于上层业务，它是基于多种算法去决策视频流中每个分片的清晰度档位，即使能够通过肉眼看出播放过程中的清晰度变化，也无法直观的确定该清晰度变化是否合理；所以我们通常通过智能档相关内核日志，确定每个分片的档位清晰度及其决策依据；测试时需要从日志中获取大量信息来判断整体决策过程是否正确，测试非常低效，同时大量的上下文信息切换，疏漏无法避免，智能档测试挑战重重；

2. 测试痛点 & 难点：智能档涉及算法众多，算法参数配置复杂（常用算法参数配置超过 20 余种）；网络环境模拟难（限速、丢包、延时、网络秒级控制）；校验标准量化难（算法逻辑复杂，每一次决策都有多种因素影响）；日志量庞大（一集 40 分钟的视频需要关注上千个关键信息）；

3. 智能档播测解决方案：

   第一步，用户行为模拟： AFrame 作为一个独立模块打包到被测 app，通过获取当前播放实例，以白盒方式调用播放器内部的各种 API，模拟用户对播放器的各种操作；

![434](https://static001.infoq.cn/resource/image/ab/f0/abf99fdc7a8cd166ebe74d5242f24ff0.png)

第二步，测试环境模拟：对于网络环境模拟，将路由器与一台主机 A 的网卡连接，通过网卡命令控制连接到路由器的移动设备网速、丢包、延迟。将 TCP Server 搭建到主机 A，任意一台 PC/MAC 均可通过限速 TCP Client 对网络进行秒级控制。限速网络 json 配置以 _{“50”: [1500, 0, 0]}_ 为例，含义为 50 秒时，限速为 1500kbps，丢包为 0%，延迟为 0 毫秒；

测试配置方面，优酷 app 使用 APS 和 Orange 进行线上配置下发，常规测试方法中，我们通过扫码方式手动拉取配置 且在 app 重启后反复操作拉取。在播测方案中，通过 AFrame 提供的 APS 和 Orange Hook 能力，直接通过 API 接口完成对 APS 和 Orange 的配置，例如配置智能档使用特定的决策策略，只需在测试用例中加上 _APSConfig.setAPSConfig(commandSender, “adaptive_bitrate”, “[“source_adaptive_mode=5”]”)_ 即可；

第三步，全流程自动化：目前基于 AFrame 测试框架和限速网络，已实现一键式执行智能档日常回归测试，用例执行结束后在播放器统一测试平台上生成测试报告。智能档播测流程如下所示：

![优酷如何构建覆盖全网的播放白盒测试体系](https://static001.infoq.cn/resource/image/05/6f/058043b4e23874d1f53f066dcd985c6f.png)

#### 基于外网环境的拨测能力

实验室网络和外部网络的最大区别是网络环境的复杂性和透明度，在实验室环境下通过网络控制来模拟各类场景，在确定的环境下执行播放测试任务，由于清楚的知道实验室网络各项指标，测试的预期结果相对比较明确；但由于播放链路的复杂性和多样性，仅在实验室环境无法有效覆盖真实情况，因此基于外网环境的拨测能力是对实验室播测方案的有效补充，在外部网络中，我们无法准确获取网络状态，因此我们结合自研的网络探测能力，获取不同域名下的网络带宽、时延、丢包率、连接速度等信息，通过计算其平均值、标准差、变异系数进行动态的 case 评估校验。从技术上来讲，外网拨测也是基于 AFrame 框架来实现的，通过 AFrameServer 外网部署，支持外部设备数据的接收和中转，结合网络探测能力，实现基于网络场景的动态拨测验证，外网拨测针对网络调度、播放链路相关的潜在问题具备较好的发现能力，由于篇幅有限并且核心链路和播测方案类似，这里不再展开赘述。

## 建议实现

### 工具栈

- 构建工具 - Gradle
- 测试工具 - Junit
- 持续集成工具 - Jenkins
- 静态扫描工具 - SonarQube
- 测试覆盖率工具 - Jacoco

### 实现的功能

- 代码提交后，进行扫描，找出代码中存在的规范性和安全问题。
- 可以控制扫描规则和扫描等级。
- 静态扫描结果及时反馈给相关人，通过邮件企业微信等方式。
- 可以区分代码分支（重复代码和相同命名）。
- 收集 bug 数据，对 bug 原因进行分析，反馈给开发，提高代码质量。
- 检测单元测试覆盖率。
- 可以设置定时扫描任务和触发扫描机制（比如每天晚上扫描一次，提交代码触发扫描一次）



### 工作流

1. 本地化 IDE 中通过 SonarLint 插件实现和 SonarQube 平台规则、规范的同步、实现本地代码检查；定制化 java 规则，用于检测代码的规范性、代码缺陷、漏洞、坏味道、重复率等信息。并将定制化的规则放到 SonarQube 平台上，SonrLint 插件的规则比较全面，为了保证本地使用定制的规则，且同 SonarQube 中的规则一致，远程连接 SonarQube 服务器，并绑定项目。
2. 代码提交到代码库 GitLab 中后，在 Jenkins 中测试环境构建时，自动触发 Sonar 扫描，并将扫描结果发布到 SonarQube 平台。

3. SonarQube 平台根据质量阀的要求，不满足质量阀要求则邮件通知开发人员。

4. 开发人员收到邮件后，进行代码处理。

5. 以周为单位统计 SonarQube 平台中静态代码扫描出来的 Bugs \漏洞\坏味道的数量，定时自动发送周报给相关干系人，报告中会包含问题处理情况的趋势图（以天为单位）。





### 交互参考

**业务信息**

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/94259e71-1248-4656-a323-ae06bd9be240.png!large)

**新增业务**

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/6a2b4740-d171-4fbb-8356-0315547ca8d4.png!large)

**业务代码质量度量**

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/e401a437-c192-4995-868c-e3759c3b28a9.png!large)

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/b5411751-3390-44b6-b48c-dc0d721366e1.png!large)

**趋势图**

![img](https://testerhome.com/uploads/photo/2020/a6eae4d9-4d48-441d-8ae7-cca23a591687.png!large)

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/d8f86236-66b5-488e-bcdc-b201361b4800.png!large)

**度量指标**

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/c85f19ac-ee56-495e-b224-a851b9498537.png!large)

**配置扫描规则（SonarQube 页面）**

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/44905F7ADB974D96848F23DFE862749A)

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/4DEA5DB32C2545F893EDC0B4FA6B9688)

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/733A3352BE6245258A1650D2F4467D6A)

**结果报告**

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/20784029-b2990f54872c15ce.png)

https://www.sourcebrella.com/static/vid/tur/report/report_zh.mp4