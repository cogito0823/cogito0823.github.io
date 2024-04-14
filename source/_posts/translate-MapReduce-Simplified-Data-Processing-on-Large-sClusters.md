---
title: 译"MapReduce:Simplified Data Processing on Large Clusters"
date: 2023-04-14 18:12:00
mathjax: true
tags: [map,reduce,cluster,mapreduce]
kewords: [map,reduce,cluster,mapreduce]
categories: cloud_computing
---
粗译谷歌大名鼎鼎的论文 “MapReduce: Simplified Data Processing on Large Clusters”

![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/winding-road-photography-1133505.jpg)

作者：Jeffrey Dean、Sanjay Ghemawat  译者：[cogito0823](blog.ccogito.xyz) 

原文：[MapReduce: Simplified Data Processing on Large Clusters](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/16cb30b4b92fd4989b8619a61752a2387c6dd474.pdf)

<!--more-->

### 摘要

MapReduce是用于处理和生成大型数据集的编程模型及实现。用户指定map函数处理键/值对以生成一组中间键/值对，Reduce函数合并与同一中间键相关联的所有中间值。如本文所示，许多现实世界中的任务都可以在此模型中表达。

以这种函数风格编写的程序会自动并行化，并可在大型商用机器集群上执行。run-time系统负责对输入数据进行分区、调度机器执行程序、处理机器故障以及管理机器间通信等细节。这使得没有任何并行和分布式系统经验的程序员可以轻松地利用大型分布式系统的资源。

我们的MapReduce实现在大型商用机器集群上运行，并且具有高度可伸缩性：典型的MapReduce计算-在数千台机器上处理数TB的数据。程序员发现该系统很容易使用：数百个MapReduce程序已经实现，每天有超过1000个MapReduce任务在Google的集群上执行。

### 引言

在过去的五年中，作者和Google的其他许多人已经实现了数百个处理大量原始数据的特殊用途的计算，例如爬行文档、web请求日志等，以计算各种派生数据，例如倒排索引、web文档的图形结构的各种表示、每个主机爬行的页数的汇总、给定一天中最频繁的查询集合等。大多数这样的计算在概念上都很简单。然而，输入的数据通常很大，为了在合理的时间内完成，计算必须分布在数百或数千台机器上。如何并行化计算、分发数据和处理故障等问题使得最初的简单计算变得模糊不清，需要大量复杂的代码来处理这些问题。

为了应对这种复杂性，我们设计了一个新的理念，它允许我们表达我们试图执行的简单计算，但隐藏了库中并行化、容错、数据分发和负载平衡的混乱细节。我们的理念灵感来自于Lisp和许多其他函数式语言中的map和reduce原语。我们意识到，我们的大多数计算涉及对输入中的每个逻辑“记录”应用map操作，以便计算出一组中间键/值对，然后对共享相同键的所有值应用Reduce操作，以便适当地组合派生数据。我们使用具有用户指定的map和reduce操作的函数模型，使我们可以轻松地并行化大型计算，并使用重新执行作为主要的容错机制。

这项工作的主要贡献是一个简单而强大的接口，它支持大规模计算的自动并行化和分布式，并结合该接口的实现，在大型商用PC群集上实现高性能。

第2节描述了基本的编程模型，并给出了几个例子。第3节描述了为我们的基于集群的计算环境量身定做的MapReduce接口的实现。第4节描述了我们发现有用的编程模型的几个改进。第5节提供了针对各种任务的实现的性能度量。第6节探讨了MapReduce在Google中的使用，包括我们将其用作重写生产索引系统的基础经验。第7节讨论了相关的工作和未来的工作。

### 编程模型

计算获取一组输入键/值对，并生成一组输出键/值对。MapReduce库的用户将计算表示为两个函数：map和reduce。

用户编写的map接受一个输入对并生成一组中间键/值对。MapReduce库将与同一中间键i关联的所有中间值组合在一起，并将它们传递给Reduce函数。

同样由用户编写的Reduce函数接受中间键i和该键的一组值。它将这些值合并在一起，形成一个可能更小的值集。通常，每次Reduce调用只生成零个或一个输出值。中间值通过迭代器提供给用户的Reduce函数。这使我们可以处理内存无法容纳的值列表。

#### 示例

考虑计算大量文件组成的文件集合中每个单词出现的总次数的问题。用户将编写类似于以下伪代码的代码：
```java
map(String key, String value):
	// key: document name
	// value: document contents
	for each word w in value:
	    EmitIntermediate(w, "1"); 

reduce(String key, Iterator values):
    // key: a word 
    // values:a list of counts 
    int result = 0;
    for each v in values:
        result += ParseInt(v);
    Emit(AsString(result)); 
```
map函数发出每个单词加上相关的出现次数(在这个简单示例中只有‘1’)。reduce函数将map为特定单词发出的所有计数加在一起。

此外，用户编写代码时用输入和输出文件的名称以及可选的调优参数来描述规范的MapReduce对象。然后，用户调用MapReduce函数，将描述的对象传递给它。用户的代码与MapReduce库(用C++实现)链接在一起。附录A包含此示例的完整程序文本。

#### 类型

尽管前面的伪代码是按照字符串输入和输出编写的，但从概念上讲，用户提供的map和reduce函数都有关联的类型：

$\quad \text{map} \qquad (\text{k1,v1}) \qquad \qquad \to \text{list(k2,v2)} \\\\ \quad \text{reduce} \quad \text{(k2,list(v2))} \qquad \to \text{list(v2)}$

即，输入键和值来自与输出键和值不同的域。此外，中间键和值与输出键和值来自相同的域。

我们的C++实现，将字符串传入和传出用户定义函数，并让用户代码在字符串和适当类型之间进行转换。

#### 一些例子

这里有几个有趣的程序的简单例子，它们可以很容易地表示为MapReduce计算。

**分布式grep：**如果与提供的模式匹配，则map函数发出一行。reduce函数是一个标识函数，它只将提供的中间数据复制到输出。

**URL访问频率计数：**map函数处理网页请求日志，并输出$\langle \text{url，1} \rangle$。reduce函数将同一URL的所有值相加，并发出对：$\langle \text{URL,total count} \rangle$。

**反向 Web-Link 图：**map函数将名为source 的页面中找到的 $\text{target}$ URL输出为每个链接的$\left \langle \text{target,source} \right \rangle$对。Reduce 函数计算与给定 $\text{target}$ URL相关联的所有source URL的列表，并发出对：$\langle\text{target,list(Source)}\rangle$

**Term-Vector per Host：**Term-Vector将文档或一组文档中出现的最重要的单词汇总为$\langle \text{word，requency} \rangle$对的列表。map函数为每个输入文档发出一个$\langle \text{hostname,term vector} \rangle$对 (其中$\text{hostname}$是从文档的URL中提取的)。
向reduce函数传递给定hostname的每个文档的term-vector。它将这些term-vector相加，丢弃不常用的terms，然后发出最终的$\langle \text{hostname,term vector} \rangle$对。

**倒序索引：**map函数解析每个文档，并发出一系列$\langle \text{word,document ID} \rangle$对。  reduce函数接受给定单词的所有对，对相应的文档ID进行排序，并发出$\langle \text{word},list(\text{document  ID}) \rangle$对。 所有输出对的集合形成一个简单的倒排索引。 易于扩展此计算以跟踪单词位置。

**分布式排序：**map函数从每个记录中提取键，并发出一个$\langle \text{key,record} \rangle$对。  reduce函数将所有对保持不变地发出。 该计算取决于第4.1节中描述的分区功能和第4.2节中描述的排序属性。

### 实现

MapReduce接口可能有许多不同的实现。 实现的选择取决于环境。 例如，一种实现可能适用于小型共享内存计算机，另一种实现适用于与交换式以太网连接在一起的商用PC集群[4]。 在我们的环境中：

（1）机器通常是运行Linux的双处理器x86处理器，每台机器具有2-4 GB的内存。

（2）使用商品联网硬件–在机器级别通常为100兆比特/秒或1吉比特/秒，但平均对等带宽要小得多。

（3）集群由数百或数千台计算机组成，因此机器故障很常见。

（4）通过直接连接到单个计算机的廉价IDE磁盘提供存储。 内部开发的分布式文件系统[8]用于管理存储在这些磁盘上的数据。 文件系统使用复制来在不可靠的硬件上提供可用性和可靠性。

（5）用户将作业提交到调度系统。 每个作业包含一组任务，并由调度程序分配到集群中的一组可用计算机。

#### 实现概述

通过将输入数据自动划分为一组M个拆分，实现Map调用分布在多台计算机上。 输入拆分可由不同的机器并行处理。 通过使用分区函数（例如，hash（key）mod R）将中间键空间划分为R个片段，实现reduce调用分布。 分区数（R）和分区功能由用户指定。

![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/20200406124926.png)

图1显示了我们的实现的MapReduce操作的总体流程。 当用户程序调用MapReduce函数时，将发生以下操作序列（图1中的数字标签与下面列表中的数字相对应）：

1. 用户程序中的MapReduce库首先将输入文件拆分为M个文件，每个文件通常为16兆字节至64兆字节（MB）（可由用户通过可选参数控制）。 然后，它在计算机群集上启动该程序的许多副本。
2. 该程序的副本之一-master是特殊的。 其余的是由master分配工作的worker。 要分配M个map任务和R个reduce任务。 master选择空闲的worker，并为每个worker分配一个map任务或一个reduce任务。
3. 分配了map任务的worker将读取相应的被拆分后原输入文件的内容。 它从输入数据中解析键/值对，并将每对传递给用户定义的Map函数。 由Map函数产生的中间键/值对被缓存在内存中。
4. 定期将缓冲中的对写入本地磁盘，并通过分区功能将其划分为R个区域。 这些缓冲对在本地磁盘上的位置被传递回master，master负责将这些位置转发给reduce worker。
5. 当master告知reduce worker这些位置时，它将使用远程程序调用，从map worker的本地磁盘读取缓冲的数据。 当reduce worker读取了所有中间数据时，它将按中间键对数据进行排序，以便将同一键的所有出现都分组在一起。 之所以需要排序，是因为通常有许多不同的键映射到相同的reduce任务。 如果中间数据量太大而无法容纳在内存中，则使用外部排序。

6. reduce worker迭代排序过的中间数据，对于遇到的每个唯一的中间键，它将键和相应的中间值集传递给用户定义的的Reduce函数。  Reduce函数的输出将附加到此reduce分区的最终输出文件中。
7. 完成所有map任务和reduce任务后，master将唤醒用户程序。 此时，用户程序中的MapReduce调用返回到用户代码。

成功完成后，可在R输出文件中找到使用mapreduce执行后得到的输出（每个reduce任务一个，其文件名由用户指定）。 通常，用户不需要将这些R输出文件合并为一个文件–他们通常将这些文件作为输入传递给另一个MapReduce调用，或者在另一个能够处理被划分成多个文件的分布式应用程序中使用它们。

#### master数据结构

master保留几个数据结构。 对于每个map任务和reduce任务，它存储状态（*idle*, *in-progress*, or *completed*）和worker的标识（for non-idle tasks）。

master是将中间文件区域的位置从map任务传递到reduce任务的管道。 因此，对于每个completed的map任务，master存储由map任务生成的R个中间文件区域的位置和大小。 map任务完成后，master将接收到此位置和大小信息的更新。 信息被逐步推送给正在执行reduce任务的worker。

#### 容错

由于MapReduce库旨在帮助使用数百或数千台计算机处理大量数据，因此该库必须能够容忍机器故障。

##### worker 的错误

master定期对每个worker执行ping操作。 如果在一定时间内未收到来自worker的响应，则master将worker标记为`failed`。 由worker完成的所有map任务都将重置为其初始idle状态，因此有在其他worker上进行调度的许可。同样，在failed的worker上正在进行的任何map任务或reduce任务也将被重置为`idle`，并有资格进行重新调度。

完成的map任务在发生故障时会被重新执行，因为它们的输出存储在故障机器的本地磁盘上，无法被访问。 已完成的reduce任务的输出存储在全局文件系统中，因此无需重新执行。

当一个map任务首先由工作程序A执行，然后再由工作程序B执行（因为A failed）时，将向所有执行reduce任务的worker通知重新执行。 任何尚未从工作程序A读取数据的reduce任务都将从工作​​程序B读取数据。

MapReduce可以抵抗大规模的worker故障。 例如，在一次MapReduce操作期间，正在运行的集群上要进行网络维护导致几分钟内无法访问一个80个计算机所组成的组，这时，MapReducemaster只是重新执行了无法访问的worker所完成的工作，并继续取得进展，最终完成MapReduce操作。

##### master 的错误

让master制作上述master数据结构的定期检查点很容易。 如果master出错，可以从最后一个检查点状态启动一个新副本。 但是，由于只有一个master，master发生故障的可能性不大。 如果master出错，我们目前的实现是中止MapReduce计算。 客户可以检查这种情况，并根据需要重试MapReduce操作。

##### 存在故障时的语义

当用户提供的map和Reduce操作符是其输入值的确定性函数时，我们的分布式实现产生的输出与整个程序的无故障顺序执行所产生的输出相同。

我们依靠映射和缩减任务输出的原子提交来实现此属性。 每个进行中的任务都将其输出写入专用临时文件。 Reduce任务生成一个这样的文件，而map任务生成R个这样的文件(每个Reduce任务一个)。 映射任务完成后，Worker会向主服务器发送一条消息，并在消息中包含R个临时文件的名称。 如果主服务器收到已经完成的MAP任务的完成消息，它会忽略该消息。 否则，它在主数据结构中记录R文件的名称。 当Reduce任务完成时，Reduce工作器会自动将其临时输出文件重命名为最终输出文件。 如果在多台计算机上执行相同的Reduce任务，则将对同一最终输出文件执行多个重命名调用。 我们依靠底层文件系统提供的原子重命名操作来保证最终的文件系统状态只包含一次执行Reduce任务所产生的数据。

我们的绝大多数map和Reduce操作符都是确定性的，在这种情况下，我们的语义等同于顺序执行，这使得程序员很容易对他们的程序行为进行推理。 当MAP和/或Reduce操作符是不确定的时，我们提供了较弱但仍然合理的语义。 在存在非确定性运算符的情况下，特定归约任务R1的输出等同于由非确定性程序的顺序执行产生的R1的输出。 然而，不同归约任务R2的输出可以对应于由非确定性程序的不同顺序执行产生的R2的输出。

考虑映射任务M和减少任务R1和R2。 设e(Ri)是提交的Ri的执行(恰好有一个这样的执行)。 出现较弱的语义是因为e(R1)可能已经读取了M的一次执行所产生的输出，并且e(R2)可能已经读取了M的另一次执行所产生的输出。

#### Locality

在我们的计算环境中，网络带宽是一种相对稀缺的资源。 我们利用输入数据(由GFS[8]管理)存储在组成集群的机器的本地磁盘上这一事实来节省网络带宽。 GFS将每个文件划分为64 MB的块，并在不同的计算机上存储每个块的多个副本(通常为3个副本)。 MapReduce主机将输入文件的位置信息考虑在内，并尝试在包含相应输入数据副本的机器上调度地图任务。 否则，它尝试在该任务的输入数据的复制品附近调度映射任务(例如，在与包含该数据的机器位于同一网络交换机上的工作机器上)。 在集群中的大部分工作进程上运行大型MapReduce操作时，大多数输入数据都是本地读取的，不会消耗网络带宽。

#### 任务粒度

如上所述，我们将映射阶段细分为M个片段，将Reduce阶段细分为R个片段。 理想情况下，M和R应该远远大于工作计算机的数量。 让每个工作进程执行许多不同的任务可以改进动态负载平衡，还可以在一个工作进程出现故障时加快恢复速度：它已经完成的许多映射任务可以分布在所有其他工作进程机器上。

在我们的实现中，M和R可以有多大是有实际限制的，因为如上所述，主机必须做出O(M+R)调度决策并在内存中保持O(M∗R)状态。 (但是，内存使用的恒定因素很小：状态的O(M∗R)段由每个映射任务/Reduce任务对大约一个字节的数据组成。)。 

此外，R通常受到用户的限制，因为每个Reduce任务的输出最终都在一个单独的输出文件中。 在实践中，我们倾向于选择M，这样每个单独的任务大约有16MB到64MB的输入数据(这样上面描述的局部性优化是最有效的)，并且我们使R成为我们预期使用的工作机器数量的小倍数。 我们经常使用2，000台工作计算机执行M=200，000和R=5，000的MapReduce计算。

#### 任务备份

延长MapReduce操作所需总时间的常见原因之一是“掉队”：机器需要异常长的时间才能完成计算中最后几个map或Reduce任务之一。 掉队的人可能会因为一大堆原因而出现。 例如，具有损坏磁盘的计算机可能会遇到频繁的可纠正错误，这些错误会使其读取性能从30 MB/s降至1 MB/s。群集调度系统可能已在该计算机上调度了其他任务，从而由于CPU、内存、本地磁盘或网络带宽的竞争，导致其执行MapReduce代码的速度变慢。 我们最近遇到的一个问题是机器初始化代码中的一个bug，它导致处理器缓存被禁用：受影响机器上的计算速度减慢了一百倍以上。

我们有一个通用的机制来缓解掉队的问题。 当MapReduce操作接近完成时，主服务器会调度其余正在进行的任务的备份执行。 只要主执行或备份执行完成，任务就被标记为已完成。 我们对此机制进行了调优，使其通常会使操作使用的计算资源增加不超过几个百分点。 我们发现，这大大减少了完成大型MapReduce操作的时间。 例如，当禁用备份任务机制时，第5.3节中描述的排序程序完成所需的时间延长了44%。

*（我的电脑有个小bug：时不时会出现输入法是英文而不能用shift转换成中文的的情况（输入法是中英都能用的输入法），这时候要手动重新选择输入法，然后再看情况要不要用shift键。这个bug应该是电脑突然无法知悉自己的输入法是什么，要用户去确认它才会得出自己的输入法。*

*上面的情况可以简化成电脑要在用户确认（观测）的情况下才能获知自己的状态，这有点像量子效应的另一种解释了，要有观测才能有状态）*

### 改进

尽管只需编写Map和Reduce函数提供的基本功能就足以满足大多数需求，但我们发现一些扩展很有用。 本节将介绍这些内容。

####  分区函数

MapReduce的用户指定他们需要的Reduce任务/输出文件的数量(R)。 使用中间键上的分区函数跨这些任务对数据进行分区。 提供了使用散列的缺省分区函数(例如，“hash(Key)modR”)。 这往往会产生相当平衡的分区。 但是，在某些情况下，通过键的某些其他功能对数据进行分区是有用的。 例如，有时输出键是URL，我们希望单个主机的所有条目都以相同的输出文件结束。 要支持这样的情况，MapReduce库的用户可以提供特殊的分区功能。 例如，使用“hash(Hostname(Urlkey))mod R”作为分区函数会导致来自同一主机的所有URL都以相同的输出文件结束。

#### 顺序保证

我们保证在给定分区内，中间键/值对按键升序处理。 这种排序保证使得为每个分区生成排序的输出文件变得很容易，当输出文件格式需要支持按键进行高效的随机访问查找时，或者输出的用户发现对数据进行排序很方便时，这是很有用的。

#### 组合器函数

在某些情况下，每个映射任务生成的中间键有大量重复，并且用户指定的Reduce函数是可交换和关联的。 2.1节中的单词计数示例就是一个很好的例子。 由于词频往往遵循Zipf分布，因此每个映射任务将生成数百或数千条&lt;the，1&gt;形式的记录。 所有这些计数将通过网络发送到单个Reduce任务，然后由ReduceFunction将其相加以生成一个数字。 我们允许用户指定一个可选的组合器函数，该函数在通过网络发送数据之前对其进行部分合并。

组合器函数在执行映射任务每台计算机上执行。 通常使用相同的代码来实现组合器和归约功能。
 Reduce函数和组合器函数之间的唯一区别是MapReduce库如何处理函数的输出。 Reduce函数的输出将写入最终输出文件。 组合器函数的输出被写入中间文件，该文件将被发送到Reduce任务。

部分组合显著提高了某些类的MapReduce操作的速度。 附录A包含一个使用组合器的示例。

#### 输入输出类型

MapReduce库支持读取几种不同格式的输入数据。 例如，“文本”模式输入将每行视为键/值对：键是文件中的偏移量，值是行的内容。 另一种常见的受支持格式存储按键排序的键/值对序列。 每个输入类型实现都知道如何将自身拆分成有意义的范围，以便作为单独的映射任务进行处理(例如，文本模式的范围拆分确保范围拆分只发生在行边界)。 用户可以通过提供简单读取器接口的实现来添加对新输入类型的支持，尽管大多数用户只使用少量预定义输入类型中的一种。

读取器不一定需要提供从文件读取的数据。 例如，很容易定义从数据库或从内存中映射的数据结构读取记录的读取器。

以类似的方式，我们支持一组输出类型来生成不同格式的数据，并且用户代码很容易添加对新输出类型的支持。

#### Side-effects

在某些情况下，MapReduce的用户发现从他们的map和/或Reduce操作符生成辅助文件作为附加输出非常方便。 我们依靠应用程序编写者来使这些副作用成为原子和幂等效应。 通常，应用程序写入临时文件，并在文件完全生成后自动重命名该文件。
 我们不支持单个任务生成的多个输出文件的原子两阶段提交。 因此，生成具有跨文件一致性要求的多个输出文件的任务应该是确定性的。 在实践中，这一限制从来都不是问题。

#### 跳过错误记录

有时，用户代码中的错误会导致Map或Reduce函数在某些记录上确定性地崩溃。 此类错误会阻止MapReduce操作完成。 通常的做法是修复错误，但有时这是不可行的；可能错误位于源代码不可用的第三方库中。 此外，有时忽略几条记录也是可以接受的，例如，在对大型数据集进行统计分析时。 我们提供了一种可选的执行模式，在该模式下，MapReduce库检测哪些记录会导致确定性崩溃，并跳过这些记录以向前推进。

每个工作进程都安装一个信号处理程序，用于捕获分段冲突和总线错误。 在调用User Map或Reduce操作之前，MapReduce库将参数的序列号存储在全局变量中。 如果用户代码生成信号，则信号处理程序会向MapReduce主服务器发送一个包含序列号的“Last Gap”UDP数据包。 当主程序在特定记录上发现多个故障时，它指示在发出相应Map或Reduce任务的下一次重新执行时应该跳过该记录。

#### 本地执行

调试Map或Reduce函数中的问题可能很棘手，因为实际计算发生在分布式系统中，通常在数千台机器上，工作分配决策由主机动态做出。 为了帮助简化调试、性能分析和小规模测试，我们开发了MapReduce库的替代实现，该实现在本地计算机上顺序执行MapReduce操作的所有工作。 向用户提供控件，以便可以将计算限制到特定的地图任务。 用户使用特殊标志调用他们的程序，然后就可以轻松地使用他们认为有用的任何调试或测试工具(例如gdb)。

#### 状态信息

主服务器运行内部HTTP服务器，并导出一组状态页面以供人类使用。 状态页显示计算的进度，例如已完成的任务数、正在进行的任务数、输入字节数、中间数据字节数、输出字节数、处理速率等。这些页还包含指向每个任务生成的标准错误和标准输出文件的链接。 用户可以使用该数据来预测计算将花费多长时间，以及是否应该向计算中添加更多资源。 这些页面还可以用来计算出何时计算比预期慢得多。
 此外，顶层状态页面显示哪些员工失败，以及他们失败时正在处理的映射和减少任务。 此信息在尝试诊断用户代码中的错误时很有用。

#### 计数器

MapReduce库提供了一个计数器工具，用于对各种事件的发生情况进行计数。 例如，用户代码可能想要计算处理的单词总数或索引的德语文档的数量等。
 要使用此功能，用户代码创建一个命名的计数器对象，然后在Map和/或Reduce函数中适当地递增计数器。 例如：

```java
Counter* uppercase;
uppercase = GetCounter("uppercase");
map(String name, String contents):
	for each word w in contents:
		if (IsCapitalized(w)):
			uppercase->Increment();
		EmitIntermediate(w, "1");

```

来自各个工作者机器的计数器值定期传播到主机器(搭载在ping响应上)。 当MapReduce操作完成时，主控件聚合来自成功的map和duce任务的计数器值，并将它们返回给用户代码。 当前计数器值还会显示在主状态页面上，以便人们可以查看实时计算的进度。 聚合计数器值时，主程序会消除重复执行同一map或Reduce任务的影响，以避免重复计数。 (重复执行可能是因为我们使用备份任务以及由于失败而重新执行任务）。

某些计数器值由MapReduce库自动维护，例如处理的输入键/值对的数量和生成的输出键/值对的数量。

用户发现计数器工具对于检查MapReduce操作的行为非常有用。 例如，在某些MapReduce操作中，用户代码可能希望确保生成的输出对的数量正好等于处理的输入对的数量，或者确保处理的德文文档的分数在处理的文档总数的某个可容忍的分数内。

### 性能

在本节中，我们将测量MapReduce在大型机器集群上运行的两个计算上的性能。 一次计算在大约1TB的数据中搜索特定模式。 另一种计算对大约1TB的数据进行排序。

这两个程序代表了MapReduce用户编写的真实程序的一大子集-一类程序将数据从一种表示形式混洗到另一种表示形式，另一类程序从大型数据集中提取少量有趣的数据。

#### 集群配置

所有程序都在由大约1800台机器组成的集群上执行。 每台机器都有两个启用了超线程的2 GHz Intel Xeon处理器、4 GB内存、两个160 GB磁盘和一个千兆以太网链路。 这些机器被安排在两级树形交换网络中，根部大约有100-200 Gbps的聚合带宽可用。 所有机器都在同一托管设施中，因此任何一对机器之间的往返时间都不到1毫秒。

在4 GB的内存中，大约1-1.5 GB由群集上运行的其他任务保留。 这些程序在周末的下午执行，此时CPU、磁盘和网络大多处于空闲状态。

#### Grep

grep程序扫描1010条100字节的记录，搜索相对罕见的三字符模式(该模式出现在92，337条记录中)。 输入被分成大约64MB的片段(M=15000)，整个输出放在一个文件中(R=1)。

<img src="https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/20200406221354.png" style="zoom: 50%;" />

图2显示了随时间推移的计算进度。 Y轴显示扫描输入数据的速率。 随着分配给此MapReduce计算的计算机越多，速率就会逐渐加快，当分配了1764个工作时，速率会达到超过30 GB/s的峰值。 随着MAP任务的完成，速率开始下降，并在计算大约80秒后达到零。 整个计算从开始到结束大约需要150秒。 这包括大约一分钟的启动开销。 开销是由于程序传播到所有工作计算机，并且延迟了与GFS的交互，以便打开1000个输入文件集并获取位置优化所需的信息。

#### 排序

排序程序对1010个100字节的记录(大约1TB的数据)进行排序。 该程序仿照TeraSort基准测试[10]。

排序程序由不到50行的用户代码组成。 三行Map函数从文本行提取一个10字节的排序关键字，并将该关键字和原始文本行作为中间关键字/值对发出。 我们使用一个内置的标识函数作为Reduce操作符。 此函数将未更改的中间键/值对作为输出键/值对传递。 最终排序的输出被写入一组双向复制的GFS文件(即，2TB被写入为程序的输出)。 如前所述，输入数据被分割成64MB的片段(M=15000)。 我们将排序后的输出划分为4000个文件(R=4000)。 分区函数使用键的初始字节将其分隔为R个片段中的一个。

这个基准测试的分区函数内置了密钥分布的知识。 在一般的排序程序中，我们将添加一个预传递MapReduce操作，该操作将收集键的样本，并使用采样的键的分布来计算最终排序传递的拆分点。

![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/20200407000701.png)

图3(A)显示了排序程序的正常执行进度。 左上角的图表显示读取输入的速率。 该速率在大约13 Gb/s时达到峰值，然后相当快地消失，因为所有MAP任务都在200秒之前完成。 请注意，输入速率低于grep。 这是因为排序映射任务花费大约一半的时间和I/O带宽将中间输出写入其本地磁盘。 grep的相应中间产出的大小可以忽略不计。

左中图形显示了通过网络将数据从map任务发送到Reduce任务的速率。 在第一个映射任务完成后，此混洗立即开始。 图中的第一个驼峰是针对第一批大约1700个Reduce任务(整个MapReduce被分配了大约1700台机器，每台机器一次最多执行一个Reduce任务)。 计算大约300秒后，第一批Reduce任务中的一些任务完成，我们开始为剩余的Reduce任务洗牌数据。 所有的洗牌都是在计算600秒后完成的。 左下角的图表显示了Reduce任务将排序的数据写入最终输出文件的速率。 在第一洗牌周期结束和写入周期开始之间存在延迟，因为机器忙于对中间数据进行排序。 写入将以大约2-4 Gb/s的速率持续一段时间。 所有写入都在计算过程中完成约850秒。 包括启动开销在内，整个计算需要891秒。 这与TeraSort基准测试的当前最佳报告结果1057秒相似[18]。

有几点需要注意：由于我们的局部性优化，输入速率高于混洗速率和输出速率-大多数数据都是从本地磁盘读取的，并且绕过了带宽相对有限的网络。 混洗速率高于输出速率，因为输出阶段写入排序数据的两个副本(出于可靠性和可用性的原因，我们制作输出的两个副本)。 我们编写两个副本，因为这是底层文件系统提供的可靠性和可用性机制。 如果底层文件系统使用擦除编码[14]而不是复制，则写入数据的网络带宽要求将会降低。

#### 备份任务的影响

在图3(B)中，我们显示了在禁用备份任务的情况下执行排序程序。 执行流程类似于图3(A)所示，不同之处在于有一个非常长的尾部，其中几乎没有任何写入活动发生。 960秒后，除5个以外的所有Reduce任务均已完成。 然而，这最后几个掉队的人直到300秒后才完成。 整个计算耗时1283秒，运行时间增加44%。

#### 机器故障

在图3(C)中，我们显示了排序程序的执行，其中我们故意在计算几分钟后杀死了1746个工作进程中的200个。 底层集群调度程序立即在这些机器上重新启动新的工作进程(因为只有进程被终止，机器仍在正常运行)。

工作人员死亡显示为负输入率，因为之前完成的一些地图工作消失了(因为相应的地图工作人员被杀害)，需要重做。 重新执行此地图工作的速度相对较快。 包括启动开销在内，整个计算在933秒内完成(仅比正常执行时间增加5%)。

### 经验

我们在2003年2月编写了MapReduce库的第一个版本，并在2003年8月对其进行了重大增强，包括本地优化、跨工作计算机的任务执行的动态负载平衡等。从那时起，我们一直对MapReduce库在我们处理的各种问题上的广泛适用性感到惊喜。 它已在谷歌内部的多个域中使用，包括：

- 大规模机器学习问题，
- Google News和Froogle产品的群集问题，
- 提取用于生成热门查询报告的数据(例如，Google Zeitgeist)，
- 为新的实验和产品提取网页属性(例如，从大量网页语料库中提取地理位置以进行本地化搜索)，以及
- 大规模图形计算。

<img src="https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/20200406222855.png" style="zoom: 67%;" />

图4显示了随着时间的推移，签入我们的主要源代码管理系统的独立MapReduce程序的数量显著增加，从2003年初的0个增加到2004年9月下旬的近900个独立实例。 MapReduce之所以如此成功，是因为它可以编写一个简单的程序，并在半小时的过程中在1000台机器上高效运行，大大加快了开发和原型开发周期。 此外，它还允许没有分布式和/或并行系统经验的程序员轻松地利用大量资源。

<img src="https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/20200407000952.png" style="zoom: 50%;" />

在每个作业结束时，MapReduce库记录有关作业使用的计算资源的统计信息。 在表1中，我们显示了2004年8月在Google运行的MapReduce作业子集的一些统计数据。

#### 大规模索引

到目前为止，我们MapReduce最重要的用途之一是完全重写了生产索引系统，该系统生成了用于Google web搜索服务的数据结构。 索引系统将我们的爬行系统检索到的大量文档作为输入，存储为一组GFS文件。 这些文档的原始内容超过20TB的数据。 索引过程作为五到十个MapReduce操作的序列运行。 使用MapReduce(而不是索引系统的早期版本中的即席分布式过程)提供了几个好处：

- 索引代码更简单、更小、更容易理解，因为负责容错、分发和并行化的代码隐藏在MapReduce库中。 例如，当使用MapReduce表示时，计算的一个阶段的大小从大约3800行C++代码下降到大约700行。
- MapReduce库的性能足够好，我们可以将概念上无关的计算分开，而不是将它们混合在一起以避免额外的数据传递。 这使得更改索引过程变得很容易。 例如，在我们的旧索引系统中花了几个月时间进行的一项更改在新系统中只用了几天时间就实现了。
- 索引过程变得更容易操作，因为机器故障、机器速度慢和网络故障导致的大多数问题都由MapReduce库自动处理，无需操作员干预。 此外，通过向索引集群添加新机器很容易提高索引过程的性能。

### 相关工作

许多系统都提供了受限的编程模型，并使用这些受限来自动并行化计算。 例如，可以使用并行前缀计算[6，9，13]在N个处理器上以logn时间对N个元素数组的所有前缀计算关联函数。 根据我们在大型真实计算方面的经验，可以认为MapReduce是对其中一些模型的简化和提炼。 更重要的是，我们提供了可扩展到数千个处理器的容错实现。 相比之下，大多数并行处理系统只在较小的规模上实现，并将处理机器故障的细节留给程序员。

批量同步编程[17]和一些MPI原语[11]提供了更高级别的抽象，使程序员更容易编写并行程序。 这些系统与MapReduce之间的一个关键区别是，MapReduce利用受限的编程模型自动并行化用户程序，并提供透明的容错能力。

我们的局部性优化从活动磁盘[12，15]等技术中获得灵感，在这些技术中，计算被推送到靠近本地磁盘的处理元素中，以减少通过I/O子系统或网络发送的数据量。 我们在与少量磁盘直接连接的商用处理器上运行，而不是直接在磁盘控制器处理器上运行，但一般的方法是相似的。

我们的备份任务机制类似于夏洛特系统中采用的紧急调度机制[3]。 简单的紧急调度的缺点之一是，如果给定任务导致重复失败，则整个计算无法完成。 我们用跳过坏记录的机制修复了这个问题的一些实例。

MapReduce实现依赖于内部群集管理系统，该系统负责在大量共享计算机上分发和运行用户任务。 虽然不是本文的重点，但集群管理系统在精神上与其他系统(如秃鹰[16])相似。

作为MapReduce库一部分的排序功能在操作上类似于NOW-SORT[1]。 源机器(映射工作进程)对要排序的数据进行分区，并将其发送到R Reduce工作进程之一。 每个Reduce工作线程在本地对其数据进行排序(如果可能，在内存中)。 当然，Now-Sort没有用户可定义的Map和Reduce函数，这些函数使我们的库具有广泛的适用性。

River[2]提供了一个编程模型，其中进程之间通过分布式队列发送数据进行通信。 与MapReduce类似，River系统试图提供良好的平均案例性能，即使在存在由异构硬件或系统扰动引入的不一致性的情况下也是如此。 River通过仔细调度磁盘和网络传输来实现这一点，以实现平衡的完成时间。 MapReduce有一种不同的方法。 通过限制编程模型，MapReduce框架能够将问题划分为大量细粒度任务。 这些任务在可用的工作器上动态调度，以便更快的工作器处理更多任务。 受限编程模型还允许我们在接近作业结束时调度任务的冗余执行，从而在出现不一致(例如缓慢或停滞的工人)的情况下极大地缩短了完成时间。

BAD-FS[5]具有与MapReduce非常不同的编程模型，并且与MapReduce不同，它的目标是跨广域网执行作业。 然而，有两个基本的相似之处。 (1)两个系统都使用冗余执行来恢复故障造成的数据丢失。 (2)两者都使用位置感知调度来减少通过拥塞的网络链路发送的数据量。

TACC[7]是一个旨在简化高可用性网络服务构建的系统。 与MapReduce一样，它依赖于重新执行作为实现容错的机制。

### 结论

MapReduce编程模型已经在Google成功地用于许多不同的目的。 我们将这一成功归因于几个原因。 首先，该模型易于使用，即使对于没有并行和分布式系统经验的程序员也是如此，因为它隐藏了并行化、容错、局部优化和负载平衡的细节。 其次，大量的问题都可以很容易地表示为MapReduce计算。 例如，MapReduce用于为Google的生产性Web搜索服务生成数据、用于排序、用于数据挖掘、用于机器学习以及许多其他系统。 第三，我们开发了一个MapReduce实现，可以扩展到由数千台机器组成的大型机器集群。 该实现有效地利用了这些机器资源，因此适用于Google遇到的许多大型计算问题。

我们从这项工作中学到了几件事。 首先，对编程模型的限制使并行和分布式计算变得容易，并使这些计算具有容错性。 第二，网络带宽是稀缺资源。 因此，我们系统中的许多优化旨在减少通过网络发送的数据量：本地优化允许我们从本地磁盘读取数据，将中间数据的单个副本写入本地磁盘可节省网络带宽。 第三，可以使用冗余执行来减少机器速度慢的影响，并处理机器故障和数据丢失。

### 致谢

Josh Levenberg根据他使用MapReduce的经验和其他人的增强建议，在修订和扩展用户级MapReduce API方面发挥了重要作用，提供了许多新功能。 MapReduce从Google文件系统读取其输入，并将其输出写入Google文件系统[8]。 我们要感谢Mohit Aron、Howard Gobioff、Markus Gutschke、David Kramer、Shun-Dak Leung和Josh Redstone为开发GFS所做的工作。 我们还要感谢Percy Leung和Olcan Sercinoglu在开发MapReduce使用的集群管理系统方面所做的工作。 Mike Burrow、Wilson Hsieh、Josh Levenberg、Sharon Perl、Rob Pike和Debby Wallach对本文的早期草稿提供了有益的评论。 匿名的OSDI评审员和我们的牧羊人Eric Brewer就论文可以改进的领域提供了许多有用的建议。 最后，我们感谢Google工程组织内的所有MapReduce用户提供了有用的反馈、建议和错误报告。

### 参考文献

[1]   Andrea C. Arpaci-Dusseau, Remzi H. Arpaci-Dusseau, David E. Culler, Joseph M. Hellerstein, and David A. Patterson. High-performance sorting on networks of workstations. In *Proceedings of the 1997 ACM SIGMOD International Conference on Management of Data*, Tucson, Arizona, May 1997.

[2]   Remzi H. Arpaci-Dusseau, Eric Anderson, Noah Treuhaft, David E. Culler, Joseph M. Hellerstein, David Patterson, and Kathy Yelick. Cluster I/O with River: Making the fast case common. In *Proceedings of the Sixth Workshop on Input/Output in Parallel and Distributed Systems (IOPADS ’99)*, pages 10–22, Atlanta, Georgia, May 1999.

[3]   Arash Baratloo, Mehmet Karaul, Zvi Kedem, and Peter Wyckoff. Charlotte: Metacomputing on the web. In *Proceedings of the 9th International Conference on Parallel and Distributed Computing Systems*, 1996.

[4]   Luiz A. Barroso, Jeffrey Dean, and Urs Ho¨lzle. Web search for a planet: The Google cluster architecture. *IEEE Micro*, 23(2):22–28, April 2003.

[5]   John Bent, Douglas Thain, Andrea C.Arpaci-Dusseau, Remzi H. Arpaci-Dusseau, and Miron Livny. Explicit control in a batch-aware distributed file system. In *Proceedings of the 1st USENIX Symposium on Networked Systems Design and Implementation NSDI*, March 2004.

[6]   Guy E. Blelloch. Scans as primitive parallel operations. *IEEE Transactions on Computers*, C-38(11), November 1989.

[7]   Armando Fox, Steven D. Gribble, Yatin Chawathe, Eric A. Brewer, and Paul Gauthier. Cluster-based scalable network services. In *Proceedings of the 16th ACM Symposium on Operating System Principles*, pages 78– 91, Saint-Malo, France, 1997.

[8]   Sanjay Ghemawat, Howard Gobioff, and Shun-Tak Leung. The Google file system. In *19th Symposium on Operating Systems Principles*, pages 29–43, Lake George, New York, 2003.

[9]   S. Gorlatch. Systematic efficient parallelization of scan and other list homomorphisms. In L. Bouge, P. Fraigniaud, A. Mignotte, and Y. Robert, editors, *Euro-Par’96. Parallel Processing*, Lecture Notes in Computer Science 1124, pages 401–408. Springer-Verlag, 1996.

[10]  Jim  Gray. Sort benchmark home page. http://research.microsoft.com/barc/SortBenchmark/.

[11]  William Gropp, Ewing Lusk, and Anthony Skjellum. *Using MPI: Portable Parallel Programming with the Message-Passing Interface*. MIT Press, Cambridge, MA, 1999.

[12]  L. Huston, R. Sukthankar, R. Wickremesinghe, M. Satyanarayanan, G. R. Ganger, E. Riedel, and A. Ailamaki. Diamond: A storage architecture for early discard in interactive search. In *Proceedings of the 2004 USENIX File and Storage Technologies FAST Conference*, April 2004.

[13]  Richard E. Ladner and Michael J. Fischer. Parallel prefix computation. *Journal of the ACM*, 27(4):831–838, 1980.

[14]  Michael O. Rabin. Efficient dispersal of information for security, load balancing and fault tolerance. *Journal of the ACM*, 36(2):335–348, 1989.

[15]  Erik Riedel, Christos Faloutsos, Garth A. Gibson, and David Nagle. Active disks for large-scale data processing. *IEEE Computer*, pages 68–74, June 2001.

[16]  Douglas Thain, Todd Tannenbaum, and Miron Livny. Distributed computing in practice: The Condor experience. *Concurrency and Computation: Practice and Experience*, 2004.

[17]  L. G. Valiant. A bridging model for parallel computation. *Communications of the ACM*, 33(8):103–111, 1997.

[18]  Jim Wyllie.    Spsort: How to sort a terabyte quickly. http://alme1.almaden.ibm.com/cs/spsort.pdf.

### 词频

本节包含一个程序，该程序统计命令行上指定的一组输入文件中每个唯一单词的出现次数。

```java
#include "mapreduce/mapreduce.h"

// User’s map function
class WordCounter : public Mapper {
	public:
	virtual void Map(const MapInput& input) {
		const string& text = input.value();
		const int n = text.size();
		for (int i = 0; i < n; ) {
			// Skip past leading whitespace
			while ((i < n) && isspace(text[i]))
				i++;

            // Find word end
			int start = i;
			while ((i < n) && !isspace(text[i]))
				i++;
            	if (start < i)
					Emit(text.substr(start,i-start),"1");
  			}
    	}
	};
    REGISTER_MAPPER(WordCounter);

    // User’s reduce function
    class Adder : public Reducer {
        virtual void Reduce(ReduceInput* input) {
            // Iterate over all entries with the
            // same key and add the values
            int64 value = 0;
            while (!input->done()) {
                value += StringToInt(input->value());
                input->NextValue();
            }
            // Emit sum for input->key()
            Emit(IntToString(value));
        }
    };
    REGISTER_REDUCER(Adder);
    int main(int argc, char** argv) {
        ParseCommandLineFlags(argc, argv);
        MapReduceSpecification spec;
        // Store list of input files into "spec"
        for (int i = 1; i < argc; i++) {
        MapReduceInput* input = spec.add_input();
        input->set_format("text");
        input->set_filepattern(argv[i]);
        input->set_mapper_class("WordCounter");
    }
    // Specify the output files:
    // /gfs/test/freq-00000-of-00100
    // /gfs/test/freq-00001-of-00100
    // ...
    MapReduceOutput* out = spec.output();
    out->set_filebase("/gfs/test/freq");
    out->set_num_tasks(100);
    out->set_format("text");
    out->set_reducer_class("Adder");
    // Optional: do partial sums within map
    // tasks to save network bandwidth
    out->set_combiner_class("Adder");
    // Tuning parameters: use at most 2000
    // machines and 100 MB of memory per task
    spec.set_machines(2000);
    spec.set_map_megabytes(100);
    spec.set_reduce_megabytes(100);
    // Now run it
	MapReduceResult result;
	if (!MapReduce(spec, &result)) abort();
	// Done: ’result’ structure contains info
	// about counters, time taken, number of
	// machines used, etc.
	return 0;
}

```

