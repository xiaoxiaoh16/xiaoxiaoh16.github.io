---
layout: post
title: "FASTA Format"
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
https://zhuanlan.zhihu.com/p/20714540;
http://boyun.sh.cn/bio/?p=1192;
and so on

## 1. fasta文件格式介绍:

FASTA format is a text-based format for representing either nucleotide sequences or peptide sequences, in which base pairs or amino acids are represented using single-letter codes. 

A sequence in FASTA format begins with a single-line description, followed by lines of sequence data. 
The description line is distinguished from the sequence data by a greater-than (">") symbolin the first column.

It is recommended that all lines of text be shorter than 80 characters in length.

## 2. fasta文件格式内容:

fasta格式是一种非常简单的储存序列的格式，可以储存核酸序列（DNA/RNA）也可以储存蛋白质的氨基酸序列（Amino Acid sequence，简称AA序列）,主要分成2个部分。

1是以“>”为开始的一行主要储存的是序列的描述信息，包括数据库中的编号，序列名称，序列类型，剩下的是序列部分，中间，前后都可以有空,直到下一个">"格表示下一条。
序列部分按照官方文档的说明应该是小于120就行，一般70到80左右。其实实际操作中，程序处理的时候都是自动去掉空格和换行符，把序列读成1行再处理.

## 3. 举一个栗子！ For mRNA 序列

```
>gi|187608668|ref|NM_001043364.2| Bombyx mori moricin (Mor), mRNA
AAACCGCGCAGTTATTTAAAATATGAATATTTTAAAACTTTTCTTTGTTTTTA
TTGTGGCAATGTCTCTGGTGTCATGTAGTACAGCCGCTCCAGCAAAAATACCT
ATCAAGGCCATTAAGACTGTAGGAAAGGCAGTCGGTAAAGGTCTAAGAGCCAT
CAATATCGCCAGTACAGCCAACGATGTTTTCAATTTCTTGAAACCGAAGAAAA
GAAAGCATTAAGAAAAGAAATTGAGTGAATGGTATTAGATATATTACTAAAGG
ATCGATCACAATGATATATAGATAGGTCATAGATGTCAACGTGAATTTATGGA
TTTTTGTTTTCCCCTTTGTAGTACTTACTTATAGTCAGTTCTTAAATTGATTG
CAACGACAACTGTGTACTATTTTTTATATTTGGTTCGAAAAGTTGCATTATTA
ACGATTTTAGAAAATAAAACTACTTTACTTTTACACG

```
fasta格式首先以大于号“>”开头，接着是序列的标识符“gi|187608668|ref|NM_001043364.2|”，然后是序列的描述信息。
换行后是序列信息，序列中允许空格，换行，空行，直到下一个大于号，表示该序列的结束。

所有来源于NCBI的序列都有一个gi号“gi|gi_identifier”，gi号类似与数据库中的流水号，由数字组成，具有绝对唯一性。
一条核酸或者蛋白质改变了，将赋予一个新的gi号（这时序列的接收号可能不变）。

表：序列来源的数据库与对应的标识符

| Database Name数据库名称	  | Identifier Syntax 标识符        |
|-----------------------------|---------------------------------|
| GenBank                     | gb&#124;accession&#124;locus      |
| EMBL Data Library           | emb&#124;accession&#124;locus     |
| DDBJ, DNA Database of Japan | dbj&#124;accession&#124;locus     |
| NBRF PIR                    | pir&#124;&#124;entry              |
| Protein Research Foundation | prf&#124;&#124;name               |
| SWISS-PROT                  | sp&#124;accession&#124;entry name |
| Brookhaven Protein Data Bank| pdb&#124;entry&#124;chain         |
| Patents                     | pat&#124;country&#124;number      |
| GenInfo Backbone Id         | bbs&#124;number                  |
| General database identifier | gnl&#124;database&#124;identifier |
| NCBI Reference Sequence     | ref&#124;accession&#124;locus     |
| Local Sequence identifier   | lcl&#124;identifier              |

对于自己构建的序列数据库（序列不是来源与NCBI或其他数据），可以采用“gnl|database|identifier”或者“lcl|identifier”格式，以保证可以使用blast的所有功能。
database或者identifier是需要指定的数据库的标识和序列标识，指定的名称可以用大小写字母、数字、下划线“_”、破折号“-”或者点号“.”。
注意名称是区分大小写的，同时不能出现空格，空格表示序列标识符结束。

数据库中的序列标识符必须保证唯一，许多时候格式数据库是formatdb报告错误，就是因为标示符重复，还有一点需要强调的是序列不能为空，否则也会报错。

下面是一个例子，这四个序列的标识符都是唯一。

gnl&#124;H.sapiens&#124;seq1|
gnl&#124;H.sapiens&#124;seq2|
gnl&#124;M.Mus&#124;seq1|
lcl&#124;seq1|

## 4. 再举一个栗子！ For AA序列

这个例子是从UniRef数据库中下载的人类血红蛋白α亚基的序列:

```
>sp|P69905|HBA_HUMAN Hemoglobin subunit alpha OS=Homo sapiens GN=HBA1 
MVLSPADKTNVKAAWGKVGAHAGEYGAEALERMFLSFPTTKTYFPHFDLSHGSAQVKGHG
KKVADALTNAVAHVDDMPNALSALSDLHAHKLRVDPVNFKLLSHCLLVTLAAHLPAEFTP
AVHASLDKFLASVSTVLTSKYR

```

第1行, 以“>”为开始, 主要储存的是序列的描述信息:
        
\>sp|P69905|HBA_HUMAN Hemoglobin subunit alpha OS=Homo sapiens GN=HBA1

sp                         | SWISS-PROT 数据库
P69905                     | 这个序列在UniRef中的编号
HBA_HUMAN                  | 这个序列的简称,Hemoglobin subunit alpha是全称
OS=Homo sapiens            | 物种
GN=HBA1是指基因的名字为HBA1| 这个HBA1基因对应的蛋白的序列

其实根据官方文档，氨基酸都是用单字母表示，对应表如下：

A  | alanine               | P  | proline       
B  | aspartate/asparagine  | Q  | glutamine      
C  | cystine               | R  | arginine      
D  | aspartate             | S  | serine      
E  | glutamate             | T  | threonine      
F  | phenylalanine         | U  | selenocysteine      
G  | glycine               | V  | valine        
H  | histidine             | W  | tryptophan        
I  | isoleucine            | Y  | tyrosine
K  | lysine                | Z  | glutamate/glutamine
L  | leucine               | X  | any
M  | methionine            | *  | translation stop
N  | asparagine            | -  | gap of indeterminate length

## 5. 再来一个栗子，For 核酸序列

继续使用人类血红蛋白a亚基对应的mRNA序列，这个序列是从NCBI RefSeq数据库中下载的。
```
>gi|13650073|gb|AF349571.1| Homo sapiens hemoglobin alpha-1 globin chain (HBA1) mRNA, complete cds
CCCACAGACTCAGAGAGAACCCACCATGGTGCTGTCTCCTGACGACAAGACCAACGTCAAGGCCGCCTGG
GGTAAGGTCGGCGCGCACGCTGGCGAGTATGGTGCGGAGGCCCTGGAGAGGATGTTCCTGTCCTTCCCCA
CCACCAAGACCTACTTCCCGCACTTCGACCTGAGCCACGGCTCTGCCCAGGTTAAGGGCCACGGCAAGAA
GGTGGCCGACGCGCTGACCAACGCCGTGGCGCACGTGGACGACATGCCCAACGCGCTGTCCGCCCTGAGC
GACCTGCACGCGCACAAGCTTCGGGTGGACCCGGTCAACTTCAAGCTCCTAAGCCACTGCCTGCTGGTGA
CCCTGGCCGCCCACCTCCCCGCCGAGTTCACCCCTGCGGTGCACGCCTCCCTGGACAAGTTCCTGGCTTC
TGTGAGCACCGTGCTGACCTCCAAATACCGTTAAGCTGGAGCCTCGGTGGCCATGCTTCTTGCCCCTTTG
G
```
对于第1行，所有来自于NCBI的序列都有一个gi号，就是数据库中的流水号，具有唯一性。
gb|AF349571.1是genebank编号的信息。后面就是对序列的一个通俗的描述。这里使用的是mRNA，包含完整的CDS（Coding sequence）区域。

为什么mRNA的序列还是用T来表示，而不是U？

其实这是为了保证数据的统一性，因为U只是在RNA中替换了原来的T，所以为了下游的方便分析处理，无论RNA序列还是DNA序列都是使用T而不是U。对于核苷酸序列的信息，我们一般使用下面的对应表。

A | adenosine          C | cytidine             G | guanine
T | thymidine          N | A/G/C/T (any)        U | uridine 
K | G/T (keto)         S | G/C (strong)         Y | T/C (pyrimidine) 
M | A/C (amino)        W | A/T (weak)           R | G/A (purine)        
B | G/T/C              D | G/A/T                H | A/C/T      
V | G/C/A              - | gap of indeterminate length

基本上FASTA这个格式主要是把序列储蓄到数据库中的一种格式，但是它不适合储存我们刚刚测到的测序数据。一个很重要的原因就是，它没有序列的质量信息。
那一般带有测序质量信息的FASTQ格式就成了储存测序数据的常用格式啦！


## 6. UCSC DNA文件中大写字母，小写字母和Ns的意义? 

RepeatMasker和Tandem重复查找器（12个或更少的时间段）的重复显示为小写;非重复序列显示为大写。” 这是README.txt文件中的相关内容。 
在IUPAC符号表中提到的核苷酸碱基，其中最常见的是ACGT和U（通常在RNA中），'N'符号用于表示序列基序中的碱基模式，其中可以找到4个中的任何一个，或者在测序运行中，在实际碱基不能被明确确定的碱基位置。

小写字母表示重复屏蔽的区域。 N代表差距。
请参阅： https ： //groups.google.com/a/soe.ucsc.edu/d/msg/genome/S4Sx8UdJAwM/tLTpVVzdhFMJ

N的数量如何确定？
根据我的经验，一连串的N，如30或50或100，用于表示两个映射的重叠群之间的差距，但是现在，对于NGS定序子，我认为它们通常代表无法明确确定的基础，因此对于长度测量的目的我们知道片段中的碱基数量，但不知道每个碱基的实际序列。

帽中的序列通常是感兴趣的区域，例如外显子。 DNA字母表中的N表示“未知核苷酸”当实际的潜在基础未知时，它可以指任何A / T / C / G。

UCSC和GenBank文件之间的区别?
如果您从GenBank下载人类染色体数据，您将无法找到小写字母（即它们不是标准功能）。 他们在UCSC文件中的原因是因为这些文件是在他们的Genome Browser中使用的文件，它显示了这些重复

相关链接: https://askchina.me/q/Ucsc-dna-ns-36081270912
