---
layout: post
title: "How to download data from EGA"
categories: Linux基础
tags:  总结笔记 
keywords: wget, download.sh, md5
author: xiaoxiao
description: 
---

* content
{:toc}

I am going to download the dataset EGAS00000000051 from paper "A small-cell lung cancer genome with complex signatures of tobacco exposure"

It took me quit long to get the paper data


# 1. The documents will need:

- MTA (My college' agreement)
- EGA Agreement
- EGA Account

# 2. Legacy datasets 

- It cannot be downloaded through the client, and instead we should use either Aspera or FTP.

- download Aspera client

- They will give you FTP/Aspera download box, including:

- sever: xfer.crg.eu
- user: xiaoxiao
- password: hello

# 3. How to use Aspera

- wget ftp://xiaoxiao:hello@xfer.crg.eu/download.sh

- wget ftp://xiaoxiao:hello@xfer.crg.eu/dbox_content

- wget ftp://xiaoxiao:hello@xfer.crg.eu/unencrypted_MD5SUMS

- change the path int the download.sh

- ./download.sh

- I don't recommand use wget to get data. Somehow, the file will be not properly transferred to your premises

# 4. Decrypt downloaded files

- download the .jar file 

- run command: Java -jar decryptor-1.0.0.jar passed.txt NCI-H209_Normal-600-800-1_45.tar.cip

# 5. Decryption key

- A one-time link where you will find your decryption key in  the alternative email you stated.

# 6. Compare and Contrast

- md5sum command

# 7. Notes

![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/download.sh.png) 



