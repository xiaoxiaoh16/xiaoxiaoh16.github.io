---
layout: post
title: "How to draw venn diagram in R"
categories: 编程之魅:[R]
tags:  总结笔记 
keywords: venn diagram
author: xiaoxiao
description: 
---

* content
{:toc}

今天要统计用4种方法得到结果的交集情况，朋友介绍用R包画韦恩图(venn diagram)，简单快捷，还省心，棒棒哒.

# 1. how to install venn package

- install.packages("VennDiagram") //本地安装，需要软件包的完整目录
- install.packages("VennDiagram", repos="http://R-Forge.R-project.org") //指定软件包所在的网址进行安装
- library(VennDiagram) //加载软件包

# 2. 设置工作目录

- setwd('/Users/Xiaoxiaoh/Documents/2018-bioinformatics/R')
- getwd() //返回当前工作目录
- dir.create() //创建新目录

# 3. venn.diagram 函数介绍

![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/venn-diagram-function.png) 

- 参数说明
- x, a list of vectors, eg: list(gatk_sc=gatk_sc,sen_sc=sen_sc,sentieon=sentieon,gatk=gatk)
- filename: 设置图形输出文件名, 如果设置为NULL会返回网格对象本身
- height: 设置输出图形的高度，以unit为单位
- width： 设置输出图形的宽度，以unit为单位
- units: 设置输出图形的单位unit大小, eg: px
- resolution: 输出图形的清晰度，DPI数值
- imagetype: 输出图形的格式，eg: tiff, png, svg 等
- compression: 设置最终用于tiff图形的压缩算法，eg:lzw
- alpha: 设置每个区块的透明度
- main: 图形标题
- main.fontface: 字体样式，eg: 斜体，粗体等
- main.fontfamily: 字体，eg: Time New Roman等
- col: 给出每个圆的圆周的颜色, eg: transparent
- fill: 给出每个圆内的颜色，a list of colour, eg: fill = c("red", "green", "yellow", "blue")
- scaled: Enable scaling for two-set and certain three-set Euler diagrams. (euler.d must be true to enable this)
- ext.text: 当区域很小时允许外部文本框
- ext.line.lwd: Width of line connecting to ext.text
- ext.dist: Vector of length 1 or 2 indicating length of external line (use negative values to shorten the line )
- ext.length: Vector of length 1 or 2 indicating the proportion of the external line that is drawn from the anchor to the text
- ext.pos: A vector (length 1 or 2) giving the positions (in degrees) of the external area labels along the circles, with 0 (default) at 12 o'clock
- inverted: Flip the two-set Venn diagram along its vertical axis (distinguished from reverse) 
- cex: Vector giving the size for each area label (length = 1/3/7/15 based on set-number)
- cat.cex: Vector giving the size for each category name
- sub: 设置图形的子标题
- sub.cex: 设置字标题的字体大小
- main.cex: 设置主标题的字体大小
- ......

# 4. 举个栗子
![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/venn-diagram-tiff.png) 

![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/C1513_1.venn.tiff) 

# 备注


