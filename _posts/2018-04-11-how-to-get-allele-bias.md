---
layout: post
title: "how to get allel bias"
categories: 生物信息
tags:  学习笔记
keywords: allele bias
author: xiaoxiao
description: 
---

* content
{:toc}


## 1. what is allele bias
单细胞Whole-genome sequencing需要amplify，这个过程中会出现很bias,为了检验测序后的bias的概率，可以选择用dbSNP heterozygous, 这样我们就可以知道约接近50%，说明测序的bias约小。 

## 2. how to get allele bias
两种方法：
第一，利用haplotypecaller的输出vcf结果，过滤得到bulk中的dbsnp, 然后与varscan2的结果取交集得到每个单细胞中的dbsnp heterozygous, 得到的figure会在边上出现20%的cutoff
第二，仅利用haplotypecaller的vcf结果，首先过滤bulk中的dbsnp heterozygous, 然后取bulk与每个单细胞的dbsnp heterozygous，这种个人感觉更好些

## 3. 相应的代码

```
# get single cell or bulk dbSNP heterozygous
grep -v '#' Exp_1.mm.ht.vcf | awk 'length($4)==1 && length($5)==1 && $7=="." && $3!="."' | \
# for single cell or bulk depends on the column ID 
awk 'substr($10,1,3)=="0/1"'| \
awk '{print $1 "\t" $2 "\t" $2 "\t" $4 "\t" $5 "\t" $3 "\t" $10}'|sort -k1,1 -k2,2n | uniq > Exp_1_1.snp.hs.ht.vcf.11
awk '{print $1 "\t" $2 "\t" $2 "\t" $4 "\t" $5 }' Exp_1_1.snp.hs.ht.vcf.11 |sort -k1,1 -k2,2n | uniq > Exp_1_1.snp.hs.ht.vcf.5col
awk '{print $7}'  Exp_1_1.snp.hs.ht.vcf.11 >  Exp_1_1.snp.hs.ht.vcf.33
cut -d : -f 2  Exp_1_1.snp.hs.ht.vcf.33 > Exp_1_1.snp.hs.ht.vcf.44
cut -d , -f 1  Exp_1_1.snp.hs.ht.vcf.44 > Exp_1_1.snp.hs.ht.vcf.refcov
cut -d , -f 2  Exp_1_1.snp.hs.ht.vcf.44 > Exp_1_1.snp.hs.ht.vcf.mulcov
paste Exp_1_1.snp.hs.ht.vcf.5col Exp_1_1.snp.hs.ht.vcf.refcov Exp_1_1.snp.hs.ht.vcf.mulcov > Exp_1_1.snp.hs.ht.vcf
					 
# depth >= 20
awk '($6+$7)>19' Exp_1_1.snp.hs.ht.vcf > Exp_1_1.depth20.snp.germline.hs.ht.vcf
					 
# get overlap of single cell and bulk
bedtools intersect -f 1 -wa -a  Exp_1_1.snp.hs.ht.vcf -b Exp_1_1_bulk.depth20.bulk.snp.germline.hs.ht.vcf | sort -k1,1 -k2,2n | uniq > Exp_1_1.snp.germline.bed
					 
# depth >= 20
awk '($6+$7)>19' Exp_1_1.snp.germline.bed > Exp_1_1.depth20.snp.germline.bed

```

## 备注
