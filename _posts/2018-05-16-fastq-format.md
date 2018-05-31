---
layout: post
title: "FASTQ Format"
categories: 生物信息
tags:  笔记
keywords: 笔记
author: xiaoxiao
description: 
---

* content
{:toc}

网上搜索的拾遗笔记，感谢各路大神，感谢互联网！

参考链接：
https://zhuanlan.zhihu.com/p/20714540
and so on

## 1. fastq文件格式介绍:

FASTQ format stores sequences and Phred qualities in a single file. It is concise and compact. 
		        
FASTQ is first widely used in the Sanger Institute and therefore we usually take the Sanger specification and the standard FASTQ format, or simply FASTQ format
Although Solexa/Illumina read file looks pretty much like FASTQ, they are different in that the qualities are scaled differently.
						        
In the quality string, if you can see a character with its ASCII code higher than 90, probably your file is in the Solexa/Illumina format.

## 2. fastq文件格式内容:

Fastq是测序数据下机格式，其中包含测序序列(reads)的序列信息及其对应的测序质量信息。

FASTQ格式文件中每个read由四行描述, 下面是一个Illumina平台测序的真实数据,pair-end:

```
[xhao@loginnode1 data]$ head -n 4 H_1_1-r1.fq
@ST-E00522:341:HK72LCCXY:2:1101:11089:1502 1:N:0:NGCATACA
NAATATTTTAAAAAATGTTCAGTCACTGAGTTGAGGGTAAAAACTTGGCTGCTCTGGCCCTCAGGCCAGGTGCCAAGCCAGATAGATCCCACCCTTCTCTGGTCCTGGGCATTCAGACCACTCCATGTATGTAATGACTGGACAACCCGT
+
#AAFFFFJJFJJJJJJJJJJJJA-J<7-FJF7F<<AF-F-FFA-<JF<7-AFF-<FJJ-7A-7777A<A----7A---7--A----7-77---7-<-F-<A7-----7-7---77-F-7<F77F---<AFFA-A---<--)-)--77--7

[xhao@loginnode1 data]$ head -n 4 H_1_1-r2.fq 
@ST-E00522:341:HK72LCCXY:2:1101:11089:1502 2:N:0:NGCATACA
NAGTGGGAACTGGGTAAGACTGAATCATTAATGACAATTCTTGGTTGCTGATAGTGAAGGAGTCTGGAAATGTGGAGGGACAGACGGGACATCAAGAGGCAATTACGGATGGTTCATCCATTACATACAGGGAGGCGTGTGCATGCCAAG
+
#AA<FF-77-7<FFA--<FFJJJJF-JJFJ<----<-A-7F-F--7--<<JJFJ-A-FFJA-<-FJJ--7FJJ-----7-7-A--7-7-------7-7-))-----7A--77---77---A-7<<A7F-AF7))-)-)))7<---)<--<

```
FASTQ格式储存的序列信息，每1条reads的信息，可以分成4行

第1行以“@”开头，随后为Illumina 测序标识符(Sequence Identifiers)坐标等信息和描述文字(选择性部分):

@ST-E00522:341:HK72LCCXY:2:1101:11089:1502

@		  | 开始的标记符号       
ST-E00522 | 测序仪唯一的设备名称 |    the unique instrument name
341       | the run id           |
HK72LCCXY | the flowcell id      |
2         | lane的编号           | flowcell lane
1101      | tail的坐标           | tile number within the flowcell lane
11089     | 在tail中的X坐标      | 'x'-coordinate of the cluster within the tile''
1502      | 在tail中的Y坐标      | 'y'-coordinate of the cluster within the tile
	
1:N:0:NGCATACA

1        |  the member of a pair, 1 or 2 (paired-end or mate-pair reads only)               
N        |  Y if the read fails filter (read is bad), N otherwise
0        |  0 when none of the control bits are on, otherwise it is an even number
NGCATACA |  index sequence
	
第2行, 测序得到的碱基序列,一般用ATCGN来表示，其中N表示荧光信号干扰无法判断到底是哪个碱基.
	
第3行, 以“+”开始, 随后为Illumina 测序标识符(选择性部分)，一般是空的.
	
第4行, 储存的是质量信息，与第2行的碱基序列是一一对应的，其中的每一个符号对应的ASCII值成为phred值，可以简单理解为对应位置碱基的质量值，越大说明测序的质量越好。不同的版本对应的不同.

## 3. FASTQ质量值的计算方法?

在测序仪进行测序的时候，会自动根据荧光信号的强弱给出一个参考的测序错误概率（error probility，P）根据定义来说，P值肯定是越小越好。
我们怎么储存他们呢？直接储存成小数点？比如1%储存成0.01？这肯定是不高效的，因为1个碱基的信息，占用了至少4个字符。
以科学家们的做法想了一个办法：

3.1) 将P取log10之后再乘以-10，得到的结果为Q。
> 比如，P=1%，那么对应的Q=-10*log10（0.01）=20

3.2) 把这个Q加上33或者64转成一个新的数值，称为Phred，最后把Phred对应的ASCII字符对应到这个碱基。
> 如Q=20，Phred = 20 + 33 = 53，对应的符号是”5”

这样就可以用1个符号与1个碱基一一对应, so smart!

所以，质量值(Phred)越大测序错误率(e)越低，准确性越高.

## 4. 各版本不同Phred对应的ASCII值

在计算Q值和加上33/64的时候，不同测序仪，产生的数据不同，大概如下所示：

solexa标准:     Q[Solexa] = (-10) * log10[(P/1-P)]

Illumina标准:   Q[Illumina]= (-10) * log10P

不同测序仪的不同Phred值对应的ASCII表:

![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/Q2ASCII.png)

目前illumina采用的都是phred + 33

质量字符的ASCII值和质量得分的关系有如下两种：
Phred+64 质量字符的ASCII值 - 64
Phred+33: 质量字符的ASCII值 - 33

可以粗略分为 Phred+33和Phred+64，这里的33和64就是指ASCII值转换为质量得分该减去的数值。

在处理测序数据时，因为一些软件会根据碱基质量得分的不同做不同的处理，常要指定正确的编码方式，有必要对质量字符与质量得分的关系（Phred+33或Phred+64）作出正确的判断。
当然，如果处理的是最近两年产生的测序数据，基本上都是Phred+33的，但从NCBI SRA数据库下载的旧数据就不一定了。

## 备注
