---
layout: post
title: "How to draw line in R"
categories: 编程之魅
tags:  R 学习笔记 
keywords: 折线图／走势图  
author: xiaoxiao
description: 
---

* content
{:toc}




## 1.plot函数用来画折线图或者走势图.

有时，图上的点并不能很清晰的让我们看出它代表多少数值，就可以用text函数来帮助我们做这件事，我们可以在每个点处标出这个点代表了多少数值。

> plot(x, y, type = "b", pch = 21, col = "red", yaxt = "n",lty = 3, ann = FALSE)

某些参数介绍：

- X：X轴的数据

- y：y轴的数据

- type主要有3中取值：:"p"只显示数据点，不显示数据点之间的连线; "l" 只显示走势线，不单独标识出点; "b" 既显示线也标识出数据点

- pch:用于指定绘制点时使用的符号取值从1-24

- lty:线条的类型，取值1-6					

- yaxt(or xaxt):设置为“n”则Y(X)轴不显示标尺，有框架线但去除了刻度

- ann=FALSE表示移除默认的标签。

- 下面介绍参数xllm/ylim。

- 它们分别设定x轴的范围和y轴的范围（limit）。这个参数在plot中可以用，但是你使用?plot查看帮助却找不到该参数，因为它实际上在函数plot.window中
						
## 2.text函数：用于给数据点加标注的，可以加你想加的内容

> test(location,pos)

参数介绍：

- location为一对X,Y坐标，不用另外加小括号将X,Y括起来，直接用逗号隔开就行	

- pos是标注在指定坐标点的四周的哪个方向 1=下 2=左 3=上 4=右

## 3.axis函数用来添坐标轴(包括坐标轴，刻度线和刻度线标签)

> axis(side, at = NULL, labels = TRUE, tick = TRUE, line = NA, pos = NA, outer = FALSE, font = NA, lty = "solid", lwd = 1, lwd.ticks = lwd, col = NULL, col.ticks = NULL, hadj = NA, padj = NA, ...)

参数说明:

- side确定添加哪个轴，其中1为x轴，2为y轴，3为上面的轴，4为右面的轴。

- at确定坐标轴刻度线添加的位置。at也可以不赋值，此时，刻度线会根据默认的算法添加

- labels确定坐标轴刻度线标签的名称。当at确定后，若labels没有赋值，则会把at的值赋给刻度线标签

- tck确定刻度线的长短，其中负数为向下，整数向上，越大越长

- line可以水平移动坐标轴的位置，负的向正方向移动，正的向负方	

## 4.grid 对图添加栅格

> grid(nx=NA,ny=6,lwd=2)

## 5.legend函数用来向图中添加图例

注意的是可以调用locator(1)函数来替换参数x和y

> legend(x, y = NULL, legend, fill = NULL, col = par("col"), border = "black", lty, lwd, pch, angle = 45, density = NULL, bty = "o", bg = par("bg"), box.lwd = par("lwd"), box.lty = par("lty"), box.col = par("fg"), pt.bg = NA, cex = 1, pt.cex = cex, pt.lwd = lwd, xjust = 0, yjust = 1, x.intersp = 1, y.intersp = 1, adj = c(0, 0.5), text.width = NULL, text.col = par("col"), text.font = NULL, merge = do.lines && has.pch, trace = FALSE, plot = TRUE, ncol = 1, horiz = FALSE, title = NULL, inset = 0, xpd, title.col = text.col, title.adj = 0.5, seg.len = 2)

参数说明:

- x, y: x,y用于定位图例，也可用单键词"bottomright", "bottom", "bottomleft", "left", "topleft", "top", "topright", "right" and "center"

- legend: 字符或表达式向量

- fill: 用特定的颜色进行填充

- col: 图例中出现的点或线的颜色

- border: 当fill = 参数存在的情况下，填充色的边框

- lty, lwd: 图例中线的类型与宽度

- pch: 点的类型

- angle: 阴影的角度

- density: 阴影线的密度

- bty: 图例框是否画出，o为画出，默认为n不画出

- bg: bty != "n"时，图例的背景色

- box.lty, box.lwd, box.col

- bty = "o"时，图例框的类型，box.lty决定是否为虚线，box.lwd决定粗线，box.col决定颜色

- pt.bg: 点的背景色

- cex: 字符大小

- pt.cex: 点的大小

- pt.lwd: 点的边缘的线宽

- x.intersp: 图例中文字离图片的水平距离

- y.intersp: 图例中文字离图片的垂直距离

- adj: 图例中字体的相对位置

- text.width: 图例字体所占的宽度

- text.col: 图例字体的颜色

- text.font: 图例字体

- merge: logical, if TRUE，合并点与线，但不填充图例框，默认为TRUE

- trace: logical; if TRUE显示图例信息.

- plot: logical. If FALSE不画出图例

- ncol: 图例中分类的列数

- horiz: logical; if TRUE,水平放置图例

- title: 给图例加标题

- inset: 当图例用关键词设置位置后，inset = 分数，可以设置其相对位置

- xpd=FALSE，即不允许在作图区域外作图，改为TRUE即可，与par()参数配合使用。

- title.col: 标题颜色

- title.adj: 图例标题的相对位置，0.5为默认，在中间。0最左，1为最右。

- seg.len: lty 与lwd的线长，长度单位为字符宽度

## 6.举个栗子

```
x <- c("1","2","3","4","5","6")  
y1 <- c(6.90713, 7.39511, 6.39133, 8.20455, 5.96904, 6.37518)   
plot(x,y1,type="o",col="red", ylim = c(1, 11),lwd = 2, xaxt="n", yaxt="n",xlab = "Months", ylab = "Frequency (E-07)") 
axis(1,at=seq(1,6,1))  # 确定添加哪个轴，其中1为x轴，2为y轴，3为上面的轴，4为右面的轴; at表示在哪个点标刻度
axis(2,at=seq(1,11,1))   
	
y2 <- c(2.39155, 2.5343, 2.89138, 2.35985, 2.93739, 1.95629)
lines(y2~x,col = "blue",type = "o",lwd = 2,pch=5)
	
y3 <- c(3.87917, 3.27423, 3.00718, 3.79678, 3.26959, 2.94459)
lines(y3~x,col = "purple",type = "o",lwd = 2,pch=4)
	
y4 <- c(6.21128, 6.79028, 5.88466, 6.48912, 7.5119, 6.35211) 
lines(y4~x,col = "orange",type = "o",lwd = 2,pch=3)
	
y5 <- c(4.27061, 4.74995, 3.5964, 4.53467, 5.64476, 3.94614) 
lines(y5~x,col = "green",type = "o",lwd = 2,pch=2)
	
y6 <- c(4.44095, 4.85507, 5.00966, 5.13081, 5.35071, 4.68798) 
lines(y6~x,col = "black",type = "o",lwd = 2,pch=1)
		
# grid(ny=NA,nx=20,lwd=2)
# grid(lwd=2)
legend("top",legend=c("M15","M17","M18","N1","N2","N5"), col=c("red","blue","purple","orange","green","black",times=1),fill, lty=1, ncol = 3,lwd = 2, cex=0.6)

```
![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/lines-diagram.png)
