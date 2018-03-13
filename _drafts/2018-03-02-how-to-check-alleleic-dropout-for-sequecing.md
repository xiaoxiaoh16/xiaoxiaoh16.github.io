---
layout: post
title: "how to check allelic dropout"
categories: 生物信息
tags:  总结笔记
keywords: ADO sequecing lorenz curve R
author: xiaoxiao
description: 
---

* content
{:toc}

最近项目中whole genome sequecing samples扩增均匀度等的不同，为了得到更精确的samples信息，对每个sample进行了1G左右的全基因组测序，进行所谓的ADO检测

# 1. 如何进行所谓的ADO检测？
> step1: a-mapping-for-ado-hs.sh  //将测序得到的fastq文件进行alignment。
> step2: genomecov-bedtools.sh   //得到基因组的genomecov
> step3: reference_define_bins.py //得到已知reference(eg: b37)的规定bin长度的reference, 这里是b37-interval-10kb.bed
> step4: samtools-bedcov-interval-10kb.sh  //得到每个10kb bin的depth
> step5:  
 
# 2. what is lorenz curve



# 3. how to plot lorenz curve
> install.packages("ineq")
> library(ineq)
> setwd('/Users/Xiaoxiaoh/Documents/2017-lei-human-patient1-for-kristina-MDA')
>  


![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/venn-diagram-function.png) 

# 4. 举个栗子

```
![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/C1513_1.venn.tiff.png) 

```

# 备注

https://www.r-bloggers.com/gini-index-and-lorenz-curve-with-r/


