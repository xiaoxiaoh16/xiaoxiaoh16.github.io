---
layout: post
title: "DNA Methylation"
categories: 生物信息
tags:  笔记 
keywords: DNA Methylation
author: xiaoxiao
description: 
---

* content
{:toc}


# 1. What is DNA Methylation

1.1 什么是DNA甲基化？

- DNA 甲基化是指在DNA甲基化转移酶(Dnmt)的作用下,CpG双核甘酸中的胞嘧啶上择性地添加一个甲基原子团(CH3)的过程.

- DNA甲基化作为DNA序列的修饰方式，能在不改变DNA序列的前提下，改变遗传表现。

- DNA甲基化可以理解为一把锁，凡是DNA甲基化标记的部分，大多是需要“尘封”，“监控”的基因，使得基因失去活性。

- DNA甲基化的主要有3种形式：5-mC （ 5－甲基胞嘧啶）和少量的N6-mA （ N6-甲基嘌呤）及7-mG （ 7－甲基鸟嘌呤）


![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/dna_mf.png)

1.2 几个基本概念（CpG sites, CpG island, CpG island shore, CpG island shelves）

CpG sites 
- The CpG sites or CG sites are regions of DNA where a cytosine nucleotide is followed by a guanine nucleotide in the linear sequence of bases along its 5' → 3' direction. CpG is shorthand for 5'—C—phosphate—G—3' 

CpG island
- CpG islands (or CG islands) are regions with a high frequency of CpG sites. 
- Though objective definitions for CpG islands are limited, the usual formal definition is a region with at least 200 bp, a GC percentage greater than 50%, and an observed-to-expected CpG ratio greater than 60 %. 长度一般为300-3000bp的区段。

CpG island shore
- CpG island shores是2009年由Rafael A Irizarry提出，将CpG岛上游2kb定义为CpG island shores

CpG island shelves
- CpG island shelves是在2011年由Marina Bibikova等人提出的，将CpG island shores上下游2kb定义为CpG island shelves(即CpG岛上下游2kb-4kb)

![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/cpg_island.png)

1.3 不同物种的甲基化情况

在哺乳动物中CpG 以两种形式存在：
- 1)一种分散存在于DNA 序列中
- 2) 另一种呈现高度聚集状态，即CpG 岛。
在正常组织里，70% ～ 90% 分散存在的CpG 是被甲基化修饰的，而CpG 岛则是非甲基化的。

Lamda 噬菌体则是完全不甲基化，所以常用于评价BS转化率。

# 2. DNA甲基化测序方法

2.1 重亚硫酸盐测序
- 是利用未甲基化的胞嘧啶可以被亚硫酸氢钠发生脱氨基变为尿嘧啶的原理重亚硫酸盐转换结合二代测序技术是目前最精准的DNA甲基化检测方法。对基因组中未发生甲基化的胞嘧啶进行重亚硫酸盐处理，将其转换成U，经PCR 扩增后变成Ｔ，重亚硫酸盐转换对甲基化的胞嘧啶不起作用。通过结合二代测序，即可绘制出单碱基分辨率的全基因组DNA甲基化图谱。 
- 目前常用的重亚硫酸盐转换结合二代测序技术的DNA甲基化检测技术有BS-Seq和RRBS等.
- 优点：精准，能够检测单碱基水平的甲基化状态。
- 缺点：贵

RRBS(Reduced Representation Bisulfite Sequencing) 
- 利用限制性内切酶对基因组进行酶切，可以极大幅度的提高CpG区域的测序深度，与重亚硫酸盐测序(Bisulfite Sequencing, BS)相比，测序量将大大减少，并在CpG岛、启动子区域和增强子元件区域达到更高精度的分辨率。同时，利用RRBS可以实现多个样本的比对基因组分析.

 BS-Seq
- 1)先利用重亚硫酸盐转换将普通的胞嘧啶变为U，而甲基化的胞嘧啶保持不变，然后利用PCR扩增使得U变成T。对转换和扩增后的DNA序列进去测序,将得到的DNA序列与参考基因组进行比较
- 2)Bisulfite处理能够将基因组中未甲基化的C碱基与甲基化的C碱基区分开来，因此成为表观遗传学研究的经典实验方法.
- 3)将Bisulfite处理与高通量测序技术的结合的全基因组甲基化测序(BS, Bisulfite Sequencing)能够绘制单碱基分辨率的DNA甲基化图谱，可用于研究物种特定DNA区域甲基化与特定表型之间的关联，并进一步研究环境、营养以及其他因素对特定基因甲基化的影响

2.2 基于限制性内切酶的测序
- 限制性内切酶法是指利用甲基化限制性内切酶（HpaII， MspI 和 HhaI 等)在各自的识别位点对甲基化的胞嘧啶有不同的敏感性来检测CpG 的甲基化。 
- 限制性内切酶法结合二代测序的技术有 MRE-Seq，MCA-Seq，MSCC和 HELP-Seq。 
- 优点：限制性内切酶测序法成本低、高效
- 缺点：由于检测的 CpG位点 局限于酶切位点附近，基因组覆盖率低，另外还存在CpG偏好性、酶切不完全导致的假阳性等问题，使用这种方法检测DNA甲基化的研究越来越少

2.3 甲基化测序靶向富集技术采用合成寡核苷酸探针来捕获CpG岛、基因启动子区域以及其他一些显著性甲基化的区域。
- 目前还不了解。。。

# 3. BS-Seq 的分析流程

3.1 原始数据的质量控制（通常是fastq或fasta格式）
- 对于真实的数据当reads的长度增加时，测序的错误率倾向于升高。另外，reads上包含的引物会降低匹配到的基因组上的准确率。因此，有时候会对序列数据进行碱基质量分数控制和修剪引物等处理。可以利用trim-galore.

3.2 序列比对
- BS-Seq产生的序列与基因组中的原始序列存在差异，将BS-Seq产生的序列数据比对到参考基因组上。Bsmap， GSNAP软件等，常用的碱基序列对比工具：Bismark.

3.3 产生甲基化水平
- 从reads的基因组位置中获得每个胞嘧啶的甲基化reads数和非甲基reads数。然后使用公式计算某个胞嘧啶的甲基化水平.
- M/(U+M) ，U和M分别是在这个胞嘧啶上的非甲基化reads数和甲基化reads

3.4 进行相关的统计以及注释
- 可以使用methylKit，后面的数据处理所开发的R包都可以读入该软件的输出。
- methylKit可以读入bismark的输出SAM文件，但是需要一定处理就是排序，借助SAMtools就可以了

# 4. Summary
读书笔记写的很糙，仅对甲基化的概念有了大概的了解，大多数内容都是从网上摘录的，还有些内容不甚了解.
后继的相关分析和注解还在学习中，会要求自己加入更多自己的问题。。。

# 备注
留作以后内容debug。
