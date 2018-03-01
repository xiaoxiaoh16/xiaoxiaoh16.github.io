---
layout: post
title: "How to draw density and histogram in R"
categories: 编程之魅
tags:  R 总结笔记 
keywords: density histogram
author: xiaoxiao
description: 
---

* content
{:toc}

今天需要画density 和histogram图形进行相关比较。

# 1. 相关统计知识

- 核密度估计（kernel density estimation）是在概率论中用来估计未知的密度函数，属於非参数检验方法之一
- 假设我们有n个数$$x_{1}-x_{i}$$，要计算某一个数x的概率密度，核密度估计的方法如下：
<script type="text/javascript"
	src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
> $$f(x) = \frac{1}{nh}\sum_{i=1}^nK(\frac{x-x_{i}}{h})$$
- 其中K为核密度函数
- h为设定的窗口
- 原理：如果某一个数在观察中出现了，可以认为这个数的概率密度很大，和这个数比较近的数的概率密度也会比较大，而那些离这个数远的概率密度会比较小。
- 核函数

# 2. density and hist 函数介绍

## 2.1 hist 函数
```
hist(x, breaks = "Sturges",freq = NULL, probability = !freq,
   include.lowest = TRUE, right = TRUE, density = NULL, 
   angle = 45, col = NULL, border = NULL, 
   main = paste("Histogram of" , xname),
   xlim = range(breaks), ylim = NULL, xlab = xname, ylab,
   axes = TRUE, plot = TRUE, labels = FALSE,
   nclass = NULL, warn.unused = TRUE, ...)
```
部分参数解释：
- x: 绘制直方图所需的一组数据向量
- breaks: 可以设置计算分布的数据区间
- freq: 逻辑值，默认为TRUE, y轴显示的是每个区间内的频数，FALSE, 代表显示的是频率（= 频数/ 总数）
- probability:  逻辑值，和 freq 参数的作用正好相反，TRUE 代表频率， FALSE 代表频数
- lables:  显示在每个柱子上方的标签
- axes: 逻辑值，是否显示轴线
- densitty 和 angle , 用线条填充柱子

## 2.2 density
```
density(x, bw = "nrd0", adjust = 1,
	kernel = c("gaussian", "epanechnikov", "rectangular",
	"triangular", "biweight","cosine", "optcosine"),
	weights = NULL, window = kernel, width,
	give.Rkern = FALSE,n = 512, from, to, cut = 3, 
	na.rm = FALSE, ..
```
部分参数介绍
- x: 需要进行核密度估计的数据
- bw: 平滑窗口宽度
- weights: 对比较重要的数据采取加权处理
- kernel: 核的选择，接受以下7个核函数选项:
- 1) gaussian: 高斯曲线，默认选项, 在数据点处模拟正态分布
- 2) epanechnikov: Epanechnikov曲线
- 3) rectangular: 矩形核函数
- 4) triangular: 三角形核函数
- 5) biweight
- 6) cosine: 余弦曲线
- 7) optcosine

# 5. 如何在一页多图？
> par(mfrow=c(m,n))
把一个页面平分成m*n份做m*n个图，用par函数的mfrow和mfcol参数。这两个参数都是两个值的向量，表示行数和列数，但在页面上作图顺序的顺序不一样。

# 6. 如何把多组数据画到同一个图
- 可以对不同数据集进行比较

## 6.1 par(new=TRUE)函数
- 如果要求两个或者多个高级做图函数画在同一个图上，而且要求重叠的话，则可以采用par(new=TRUE)函数,在每次使用新的高级做图函数之前加上该语句。
- 需要注意的是坐标要处理好，因为这样做实际上是把多张图重叠起来，如果多张图的坐标不统一，则会出现坐标混乱的情况。此外，有些做图函数本身就有这个功能，如matplot()和hist()函数。

## 6.2 par(fig=c(x1,x2,y1,y2))函数
- 使用par()的中fig=在画布任意位置上画图。
- 在使用fig参数时，需要把画布理解成左下角为坐标(0,0)，右上角为(1,1)的一个坐标系。
- fig=c(x1,x2,y1,y2)来设置该参，x1<x2,y1<y2，x1,y1定位绘图区的左下角，x2,y2定位绘图区的右上角。
- 使用new=TRUE参数来确认是否在原画布上继续画，还重新在一张新画布上开始画。

## 6.2 有些函数本身就具有add选项

## 6.2 使用低级做图函数
- 可以在高级做图函数做的图上随意添加

### 6.2.1 points()
- points()
- 用来在一张图表上添加点，指定好对应的x和y坐标后，可以添加不同形状，颜色的点

### 6.2.2 lines()
- lines()
- 用来做一般连线图，其输入是x,y的点向量

### 6.2.3 text()
- text()
- 用于向绘图区添加标注

### 6.2.4 mtext()
- ntext()
- 图形的边界添加标注

# 7. 举个栗子
```
##########################hist#####################################################
# using bedtools get the overlap 
setwd('/Users/Xiaoxiaoh/Documents/2018-bioinformatics/R')
par(fig=c(0.5,1,0.75,1))
M1=read.table('C1513_4.sc.sen-sc.gatk.sentieon.overlap.bed')
M11 <- as.numeric(M1$V6)
M12 <- as.numeric(M1$V7)
MN1=M12/(M12+M11)
M1=density(MN1)
# plot(M1)
x1.plot<-hist(MN1, breaks=1000, main="AllOverlap: 232", xlab=" ")
	
par(fig=c(0.5,1,0.5,0.75),new=T)
M2=read.table('C1513_4.gatk_sc.overlap.part4.bed')
M21 <- as.numeric(M2$V6)
M22 <- as.numeric(M2$V7)
MN2=M22/(M22+M21)
M2=density(MN2)
# plot(M2)
hist(MN2, breaks=1000, main="OnlySen_sc&Gatk-sc: 8",xlab=" ")
	
par(fig=c(0.5,1,0.25,0.5),new=T)
M3=read.table('C1513_4.gatk.sentieon.part1.overlap.bed')
M31 <- as.numeric(M3$V8)
M32 <- as.numeric(M3$V9)
MN3=M32/(M32+M31)
M3=density(MN3)
# plot(M3)
hist(MN3, breaks=1000, main="OnlySen&Gatk: 418", xlab=" ")
	
par(fig=c(0.5,1,0,0.25),new=T)
M4=read.table('C1513_4.gatk.alone.bed')
M41 <- as.numeric(M4$V8)
M42 <- as.numeric(M4$V9)
MN4=M42/(M42+M41)
M4=density(MN4)
# plot(M4)
hist(MN4, breaks=1000, main="Gatk.alone: 26", xlab=" ")
		
###############################density################################################
par(fig=c(0,0.5,0.1,0.9),new=T)
# col=rainbow(n=4)
plot(density(MN1), col = "green",ylim=c(0,15), main="C1513_4")
lines(density(MN2), col = "red")
lines(density(MN3), col = "blue")
lines(density(MN4), col = "black")
legend("topright", legend=c("AllOverlap: 232","OnlySen_sc&Gatk-sc: 8", "OnlySen&Gatk: 418","Gatk.alone: 26"),col=c("green","red","blue","black",times=1),fill, lty=1,ncol = 1,lwd = 0.3, cex=0.8)
	
```
![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/C1513_4-density-and-hist.png) 

# 备注

