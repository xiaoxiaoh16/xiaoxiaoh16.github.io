---
layout: post
title: "how to get lorenz curze"
categories: 编程之魅
tags:  R Python
keywords: lorenz curze
author: xiaoxiao
description: 
---

* content
{:toc}

## 1. what is lorenz curze
基尼系数是量度贫富悬殊程度的标量。它的定义如下：我们首先收集社会上每一个人的总财富额，把它从少至大排序，计算它的累积函数（cumulative function），然后便可绘出图中的洛仑兹曲线（Lorenz curve）
我们先想想两个极端情况。假设社会上人人财富均等，那就给成了图中的45度直线，称平等曲线（line of (perfect) equality）；
但如财富集中在一人手中，那就绘成图中在右端的竖轴，称绝对不平均直线（line of perfect inequality）。
而图中的洛仑兹曲线乃实际分佈。A和B是图中两面积，基尼系数便是。用此定义，在人人财富均等情况下，基尼系数为0；在财富一人独占的情况下，基尼系数为1。
A/(A+B)
用此定义，在人人财富均等情况下，基尼系数为0；在财富一人独占的情况下，基尼系数为1。

## 2. 基尼系数可用简单的python函数计算

感谢@何史提大神用Python写了基尼系数的近似几何算法
有兴趣者可参econ_inequality/ginicoef.py at master 路 stephenhky/econ_inequality 路 GitHub

```
@ python
import numpy as np

def gini_coef(wealths):
	cum_wealths = np.cumsum(sorted(np.append(wealths, 0)))
	sum_wealths = cum_wealths[-1]
	xarray = np.array(range(0, len(cum_wealths))) / np.float(len(cum_wealths)-1)
	yarray = cum_wealths / sum_wealths
	B = np.trapz(yarray, x=xarray)
	A = 0.5 - B
	return A / (A+B)
```

## 3. 大概流程

> fastqc --> trim --> fastqc --> bwa-mem --> picard(conver sam to bam; sort; remove duplication)

落到10kb bin长度的reads 的个数
> $samtools bedcov b37-interval-10kb.bed $1.gatk.mapq20.bam > $1.gatk.mapq20.depth

## 4. how to get b37-interval-10kb.bed 

```
#!/usr/bin/python

import sys
import datetime

print datetime.datetime.now()

filename = sys.argv[1]

def get_bin_ref(filename, step=10000):

# get 10kb bin 
def get_bin_ref(filename, step=10000):
	"""
	:param filename: 
	:param step: 
	:return: 
	"""
	d = open(filename, "w")
	chrid = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 'X', 'Y']
	maxchr = [249250621, 243199373, 198022430, 191154276, 180915260, 171115067, 159138663, 146364022, 141213431, 135534747, 135006516, 133851895, 115169878, 107349540, 102531392, 90354753, 81195210, 78077248, 59128983, 63025520, 48129895, 51304566, 155270560, 59373566]

	# To access an item in a list, index starts from 0, but shell list from 1!!!
	i = 0
	while i < 24:
		new_line = ""
		j = 1
		while j < maxchr[i]:
			if(j + step - 1 > maxchr[i]):
				new_line = ("%s\t%d\t%d") % (chrid[i], j, maxchr[i])
			else:
				new_line = ("%s\t%d\t%d") % (chrid[i], j, (j+step-1))
			j += step
			d.write("%s\n" % new_line)
		i += 1

get_bin_ref(sys.argv[1], 10000)

print datetime.datetime.now()

```

## 5. draw lorenz curze

```
setwd('/Users/Xiaoxiaoh/Documents/2017-lei-human-patient1-for-kristina-MDA')

sample='HH1_1-12'
dat1=read.table(paste(sample,'-depth',sep=''))

#name1 <- paste("depth", 1:12, sep = "_",collapse=",")
#name2 <- paste("dat1$V", 4:15, sep = "",collapse=",")
#print(name2[1])
#em<-Reduce(as.numeric,name2)

# vector 用于列合并初始化
out <- vector()
out <- rbind(out,ineq(as.numeric(dat1[,4]),type="Gini"))
plot(Lc(as.numeric(dat1[,4])), col = col[1])
for (i in 2:12){
	tmp <- ineq(as.numeric(dat1[,i+3]),type="Gini")
	print(tmp)
	out <- rbind(out,tmp)
	lines(Lc(as.numeric(dat1[,i+3])), col = col[i])
}

name <- vector()
com <- vector()
for ( i in 1:12){
	tmp <- paste(paste("HH1", i, sep = '_'),out[i],sep = ':')
	com <- rbind(com, tmp)
}

legend("topleft",legend=com,col=c(col[1],col[2],col[3],col[4],col[5],col[6],col[7],col[8],col[9],col[10],col[11],col[12],times=1),lty=1,ncol = 1,lwd = 2, cex=0.4)

```

## 备注
