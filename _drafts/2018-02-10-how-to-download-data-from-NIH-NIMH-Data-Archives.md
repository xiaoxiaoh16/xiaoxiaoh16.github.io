---
layout: post
title: "How to download data from NIH NIMH Data Archives"
categories: Linux基础
tags:  总结笔记 
keywords: orcle SQL Developer 
author: xiaoxiao
description: 
---

* content
{:toc}

I am going to download  paper data "Different mutational rates and mechanisms in human cells at pregastrulation and neurogenesis"



# 1. The documents will need:

- MTA (My college' agreement)
- NDA Agreement
- NIMH account, complete a data access request

# 2. 

- It cannot be downloaded through the client, and instead we should use either Aspera or FTP.

- download Aspera client


# 3. How to use Aspera

- wget ftp://xiaoxiao:hello@xfer.crg.eu/download.sh



# 4. Decrypt downloaded files

- download the .jar file 

- run command: Java -jar decryptor-1.0.0.jar passed.txt NCI-H209_Normal-600-800-1_45.tar.cip

# 5. Decryption key

- A one-time link where you will find your decryption key in  the alternative email you stated.

# 6. Compare and Contrast

- md5sum command

# 7. Notes

![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/download.sh.png) 



