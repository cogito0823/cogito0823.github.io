---
title: 关于 extensibility 和 scalability 的词义辨析
date: 
mathjax: false
tags: [InfluxDB, Database]
categories: tools
---

今天阅读到阮一峰前辈的“[软件架构入门](http://www.ruanyifeng.com/blog/2016/09/software-architecture.html)”一文，其中谈到“微核架构”有良好的功能延伸性（extensibility），和较差的扩展性（scalability）。

extensibility 和 scalability 在谷歌翻译里的注释都是“可扩展性”，但阮一峰前辈在文中对这两个词分别用延伸性和扩展性表述，这引起了我对这两个词的词义的好奇，于是写下这篇博文做一次探究和记录。

![image-20201224155454993](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201224155454993.png)

<!--more-->

### extensibility

通过 *Collins English Dictionary* 查阅到，extensibility 是 extensible 的派生形式，而 extensible 的释义是 `capable of being extended` 。于是继续检索 extend 的含义

> **1.** 动词
>
> If you [say](https://www.collinsdictionary.com/zh/dictionary/english/say) that something, usually something large, **extends** **for** a particular distance or **extends** **from** one place **to** another, you are indicating its size or position.	# 延伸
>
> The caves extend for some 18 kilometres. [[VERB + *for*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-for-n_1)
>
> The main stem will extend to around 12ft, if left to develop naturally. [[VERB + *to*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-to-n_1)
>
> Our personal space extends about 12 to 18 inches around us. [[VERB amount\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-amount_1)
>
> Diyala extends from the suburbs of Baghdad to the Iranian border. [VERB *from* noun *to* noun]
>
> The new territory would extend over one-fifth of Canada's land mass. [[VERB + *over*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-over-n_1)
>
> [[Also V + *to*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-to-n_1)
>
> **同义词：** spread out, [reach](https://www.collinsdictionary.com/zh/dictionary/english/reach), [stretch](https://www.collinsdictionary.com/zh/dictionary/english/stretch), [continue](https://www.collinsdictionary.com/zh/dictionary/english/continue)  [extend 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/extend#extend__1)
>
> **2.** 动词
>
> If an object **extends from** a surface or place, it [sticks](https://www.collinsdictionary.com/zh/dictionary/english/stick) out from it.
>
> A shelf of land extended from the escarpment. [[VERB + *from*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-from-n_1)
>
> **3.** 动词
>
> If an event or activity **extends** **over** a period of time, it [continues](https://www.collinsdictionary.com/zh/dictionary/english/continue) for that time.
>
> ...a playing career in first-class cricket that extended from 1894 to 1920. [VERB *from* noun *to* noun]
>
> The courses are based on a weekly two-hour class, extending over a period of 25 weeks. [[VERB + *over*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-over-n_1)
>
> [[Also V + *to*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-to-n_1)
>
> **同义词：** [last](https://www.collinsdictionary.com/zh/dictionary/english/last), [continue](https://www.collinsdictionary.com/zh/dictionary/english/continue), [go on](https://www.collinsdictionary.com/zh/dictionary/english/go-on), [stretch](https://www.collinsdictionary.com/zh/dictionary/english/stretch)  [extend 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/extend#extend__1)
>
> **4.** 动词
>
> If something **extends** **to** a group of people, things, or activities, it includes or affects them.
>
> The service also extends to wrapping and delivering gifts. [[VERB + *to*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-to-n_1)
>
> The talks will extend to the church, human rights groups and other social organizations. [[V*to* n/-ing\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-to-n_1)
>
> His influence extends beyond the TV viewing audience. [V + *beyond*]
>
> **5.** 动词
>
> If you **extend** something, you make it longer or [bigger](https://www.collinsdictionary.com/zh/dictionary/english/big).
>
> This year they have introduced three new products to extend their range. [[VERB noun\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-n_1)
>
> The building was extended in 1500. [[*be* VERB-ed\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-n_1)
>
> ...an extended exhaust pipe. [VERB-ed]
>
> **同义词：** [widen](https://www.collinsdictionary.com/zh/dictionary/english/widen), [increase](https://www.collinsdictionary.com/zh/dictionary/english/increase), [develop](https://www.collinsdictionary.com/zh/dictionary/english/develop), [expand](https://www.collinsdictionary.com/zh/dictionary/english/expand)  [extend 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/extend#extend__1)
>
> **6.** 动词
>
> If a piece of [equipment](https://www.collinsdictionary.com/zh/dictionary/english/equipment) or [furniture](https://www.collinsdictionary.com/zh/dictionary/english/furniture) **extends**, its length can be increased.
>
> ... a table which extends to accommodate extra guests. [[VERB\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v_1)
>
> The table extends to 220cm. [[VERB + *to*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-to-n_1)
>
> **7.** 动词
>
> If you **extend** something, you make it last longer than before or end at a later [date](https://www.collinsdictionary.com/zh/dictionary/english/date).
>
> They have extended the deadline by twenty-four hours. [[VERB noun\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-n_1)
>
> ...an extended contract. [VERB-ed]
>
> **同义词：** make longer, [prolong](https://www.collinsdictionary.com/zh/dictionary/english/prolong), [lengthen](https://www.collinsdictionary.com/zh/dictionary/english/lengthen), draw out  [extend 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/extend#extend__1)
>
> **8.** 动词
>
> If you **extend** something **to** other people or things, you make it include or affect more people or things.
>
> It might be possible to extend the technique to other crop plants. [[VERB noun + *to*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-n-to-n_1)
>
> **9.** 动词
>
> If someone **extends** their [hand](https://www.collinsdictionary.com/zh/dictionary/english/hand), they stretch out their arm and hand to [shake](https://www.collinsdictionary.com/zh/dictionary/english/shake) [hands](https://www.collinsdictionary.com/zh/dictionary/english/hands) with someone.
>
> The man extended his hand: 'I'm Chuck'. [[VERB noun\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-n_1)

从释义里看出，extend 其实更接近中文意思里的延伸。

从什么地方延伸到什么地方，将什么东西延伸到什么状态。进而发展成主动地拉扯使物体伸长变大，拉扯时间使截止日期延后等，其他还有事物对人群或其他事物的影响，伸出手臂和别人握手。

于是到这里似乎可以判断 `capable of being extended` 的意思就是“延展伸长的能力”，那么 **extensibility** 自然就是“延展性”。

### scalability

用同样的方法查到 **scalability** 的释义是 `the ability of something, esp a computer system, to adapt to increased demands` —— 某种东西（尤其是计算机系统）适应不断增长的需求的能力

scalable 的释义

> **1.** 
>
> capable of being scaled or [climbed](https://www.collinsdictionary.com/zh/dictionary/english/climb)
>
> **2.** computing
>
> (of a [network](https://www.collinsdictionary.com/zh/dictionary/english/network)) able to be [expanded](https://www.collinsdictionary.com/zh/dictionary/english/expand) to [cope](https://www.collinsdictionary.com/zh/dictionary/english/cope) with increased use

第 2 条用与计算机时的释义为 `(of a network) able to be expanded to cope with increased use` —— （网络）可以扩展以应对增加的使用需求

scale 的释义

>**1.** 单数名词
>
>If you [refer](https://www.collinsdictionary.com/zh/dictionary/english/refer) to the **scale** of something, you are referring to its size or extent, [especially](https://www.collinsdictionary.com/zh/dictionary/english/especially) when it is very [big](https://www.collinsdictionary.com/zh/dictionary/english/big).	# 指物体的规模或事物的程度，特别是当改事物已经非常大的时候
>
>However, he underestimates the scale of the problem. [[+ *of*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/n-of-n_1) 
>
>You may feel dwarfed by the sheer scale of the place. 
>
>The break-down of law and order could result in killing on a massive scale. 
>
>The British aid programme is small in scale. 
>
>**同义词：** [degree](https://www.collinsdictionary.com/zh/dictionary/english/degree), [size](https://www.collinsdictionary.com/zh/dictionary/english/size), [range](https://www.collinsdictionary.com/zh/dictionary/english/range), [spread](https://www.collinsdictionary.com/zh/dictionary/english/spread)  [scale 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/scale#scale__5)
>
>**2.** See also [full-scale](https://www.collinsdictionary.com/zh/dictionary/english/full-scale), [large-scale](https://www.collinsdictionary.com/zh/dictionary/english/large-scale), [small-scale](https://www.collinsdictionary.com/zh/dictionary/english/small-scale)	# 全尺寸，大尺寸，小尺寸
>
>**3.** 可数名词
>
>A **scale** is a set of levels or [numbers](https://www.collinsdictionary.com/zh/dictionary/english/numbers) which are used in a particular system of measuring things or are used when [comparing](https://www.collinsdictionary.com/zh/dictionary/english/compare) things.	# 量级，单位，层次
>
>...an earthquake measuring five-point-five on the Richter scale. 
>
>The patient rates the therapies on a scale of zero to ten. 
>
>The higher up the social scale they are, the more the men have to lose. 
>
>**同义词：** system of measurement, [register](https://www.collinsdictionary.com/zh/dictionary/english/register), measuring system, graduated system  [scale 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/scale#scale__5)
>
>**4.** See also [sliding scale](https://www.collinsdictionary.com/zh/dictionary/english/sliding-scale), [timescale](https://www.collinsdictionary.com/zh/dictionary/english/timescale)
>
>**5.** 可数名词
>
>A pay **scale** or **scale** **of** [fees](https://www.collinsdictionary.com/zh/dictionary/english/fee) is a [list](https://www.collinsdictionary.com/zh/dictionary/english/list) that shows how much someone should be paid, [depending](https://www.collinsdictionary.com/zh/dictionary/english/depend), for example, on their age or what work they do.
>
>[British]
>
>...those on the high end of the pay scale. 
>
>A Registered Osteopath will be pleased to tell you his scale of fees before you decide on a consultation. 
>
>**6.** 可数名词
>
>The **scale** of a [map](https://www.collinsdictionary.com/zh/dictionary/english/map), plan, or model is the relationship between the size of something in the map, plan, or model and its size in the real world.	# 比例，比例尺
>
>The map, on a scale of 1:10,000, shows over 5,000 individual paths. [[+ *of*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/n-of-n_1) 
>
>**同义词：** [ratio](https://www.collinsdictionary.com/zh/dictionary/english/ratio), [proportion](https://www.collinsdictionary.com/zh/dictionary/english/proportion), relative size  [scale 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/scale#scale__5)
>
>**7.** See also [full-scale](https://www.collinsdictionary.com/zh/dictionary/english/full-scale), [large-scale](https://www.collinsdictionary.com/zh/dictionary/english/large-scale)
>
>**8.** 形容词 [ADJECTIVE noun]
>
>A **scale** model or **scale** [replica](https://www.collinsdictionary.com/zh/dictionary/english/replica) of a building or object is a model of it which is smaller than the real thing but has all the same parts and [features](https://www.collinsdictionary.com/zh/dictionary/english/feature).	# 等比例模型
>
>Franklin made his mother an intricately detailed scale model of the house. 
>
>**9.** 可数名词
>
>In music, a **scale** is a fixed sequence of musical notes, each one [higher](https://www.collinsdictionary.com/zh/dictionary/english/higher) than the [next](https://www.collinsdictionary.com/zh/dictionary/english/next), which [begins](https://www.collinsdictionary.com/zh/dictionary/english/begin) at a particular note.	# 调
>
>...the scale of C major. [[+ *of*\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/n-of-n_1) 
>
>**10.** 可数名词 [usually plural]
>
>The **scales** of a fish or reptile are the small, flat pieces of hard skin that cover its body.
>
>**11.** 复数名词 [oft *a pair of* NOUN]
>
>**Scales** are a piece of equipment used for weighing things, for example for weighing amounts of food that you need in order to make a particular [meal](https://www.collinsdictionary.com/zh/dictionary/english/meal).
>
>...a pair of kitchen scales. 
>
>...bathroom scales. 
>
>I step on the scales practically every morning. 
>
>**同义词：** weighing machine, [balance](https://www.collinsdictionary.com/zh/dictionary/english/balance), [scale](https://www.collinsdictionary.com/zh/dictionary/english/scale), weigh beam  [scale 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/scale#scale__3)
>
>**12.** 动词
>
>If you **scale** something such as a mountain or a wall, you climb up it or over it.
>
>*[written]*
>
>...Rebecca Stephens, the first British woman to scale Everest. [[VERB noun\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-n_1) 
>
>The men scaled a wall and climbed down scaffolding on the other side. [[VERB noun\]](https://grammar.collinsdictionary.com/zh/grammar-pattern/v-n_1) 
>
>**同义词：** climb up, mount, [go up](https://www.collinsdictionary.com/zh/dictionary/english/go-up), [ascend](https://www.collinsdictionary.com/zh/dictionary/english/ascend)  [scale 的更多同义词](https://www.collinsdictionary.com/zh/dictionary/english-thesaurus/scale#scale__5)
>
>**13.** 
>
>See [out of scale](https://www.collinsdictionary.com/zh/dictionary/english/out-of-scale)
>
>**14.** 
>
>See [to scale](https://www.collinsdictionary.com/zh/dictionary/english/to-scale)

在 scale 的释义里可以看出，scale 主要强调事物的大小、规模。

于是 scalability 的释义似乎就是“可以按需扩展的能力”。

### 二者的对比

从上面的探究里，我的感性认识是，**scalability** 强调根据需求的增加，对原有事物的规模进行扩展；而 **extensibility** 则没有强调要根据需求的变化来使自身本体的规模扩大。

再结合实际工作中接触到的场景，**extensibility** 更多的出现在功能的增加，提升多样性的场景；**extensibility** 则更多出现在规模的动态扩展场景里，其本身的功能没有改变。

所以 **extensibility** 描述功能时，是添加不同功能的能力；**scalability** 则是规模的动态扩展能力，体现在文中即是分布式扩展能力。
