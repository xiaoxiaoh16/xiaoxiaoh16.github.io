---
layout: post
title: "How to cat all files in diff dir"
categories: Linux基础
tags:  总结笔记 
keywords: gunzip, cat, mv
author: xiaoxiao
description: 
---

* content
{:toc}

昨天收到几个T的硬盘，大概查看了下，几十个sample，每个sample文件夹下的pari-end从不同lane得到的raw data.

## 1.如何解压缩

文件是gz压缩格式，只需要用gunzip command就可以。

- 因为是在HPC cluster上完成，所以需要读出文件夹下所有sample的名字命名的文件夹名字，执行gunzip命令搞定
sample 文件夹结构如下：

- drwx------ 2 xhao res_com_domain_users         4096 Feb  5 11:27 Gt_1
- drwx------ 2 xhao res_com_domain_users         4096 Feb  5 11:27 Gt_2
- drwx------ 2 xhao res_com_domain_users         4096 Feb  5 11:27 Gt_3

```
##############################cat gunzip.sh###########################
#!/bin/bash
#$ -j y
#$ -S /bin/bash
#$ -cwd
# Job name
#$ -N gzip
# Number of cpu cores required
#$ -pe smp 1
# RAM requirement per cpu core
#$ -l h_vmem=2G

gunzip *.fq.gz 

#############################cat gunzip.sh########################
#!/bin/bash
#$ -j y
#$ -S /bin/bash
#$ -cwd
# Job name
#$ -N qsub
# Number of cpu cores required
#$ -pe smp 1
# RAM requirement per cpu core
#$ -l h_vmem=2G

dir=./
for i in $dir/* ; do
	if [ -d "$i" ]; then
		echo $(basename "$i")
		cp gunzip.sh $(basename "$i")
		cd $(basename "$i")
		qsub gunzip.sh
		cd .. 
	fi
done

```																

## 2.如何得到pair-end数据

Raw data 是几条lane测序得到的pair-end数据，结构如下：

- -rwx------ 1 xhao res_com_domain_users 21805384382 Feb  2 02:50 Gt_1_USD1836L_HF52CY_L1_1.fq
- -rwx------ 1 xhao res_com_domain_users 21805384382 Feb  2 02:50 Gt_1_USD1836L_HF52CY_L1_2.fq
- -rwx------ 1 xhao res_com_domain_users 11000925345 Feb  2 02:34 Gt_1_USD1836L-1_H7Y7CY_L5_1.fq
- -rwx------ 1 xhao res_com_domain_users 11000925345 Feb  2 02:34 Gt_1_USD1836L-1_H7Y7CY_L5_2.fq
- -rwx------ 1 xhao res_com_domain_users 11188709852 Feb  2 02:44 Gt_1_USD1836L-1_H7Y7CY_L6_1.fq
- -rwx------ 1 xhao res_com_domain_users 11188709852 Feb  2 02:45 Gt_1_USD1836L-1_H7Y7CY_L6_2.fq
- -rwx------ 1 xhao res_com_domain_users 11496603553 Feb  2 02:46 Gt_1_USD1836L-1_H7Y7CY_L7_1.fq
- -rwx------ 1 xhao res_com_domain_users 11496603553 Feb  2 02:46 Gt_1_USD1836L-1_H7Y7CY_L7_2.fq

把每个sample文件夹下的samples按照PE reads规则，执行cat command得到最终结果

```
##########################cat cat.sh###################################### 
#!/bin/bash
#$ -j y
#$ -S /bin/bash
#$ -cwd
# Job name
#$ -N gzip
# Number of cpu cores required
#$ -pe smp 1
# RAM requirement per cpu core
#$ -l h_vmem=2G


##########################cat qsub_cat.sh################################### 
dir=./
for i in $dir/* ; do
	if [ -d "$i" ]; then
			echo $(basename "$i")
			cp cat.sh $(basename "$i")
			cd $(basename "$i")
			echo "cat $(basename "$i")*_1.fq > $(basename "$i")-r1.fq" >> cat.sh
			echo "cat $(basename "$i")*_2.fq > $(basename "$i")-r2.fq" >> cat.sh
			qsub cat.sh
			cd .. 
		fi
done

```

## 3.拷贝文件至上一级目录

方便后继访问数据进行分析

```
#########################cat qsub_mv.sh###################################
#!/bin/bash
#$ -j y
#$ -S /bin/bash
#$ -cwd
# Job name
#$ -N qsub
# Number of cpu cores required
#$ -pe smp 1
# RAM requirement per cpu core
#$ -l h_vmem=2G

dir=./
for i in $dir/* ; do
	if [ -d "$i" ]; then
			echo $(basename "$i")
			cd $(basename "$i")
			mv *-r*.fq ..
			cd .. 
	fi
done

```
