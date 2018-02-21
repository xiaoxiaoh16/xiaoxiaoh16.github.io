---
layout: post
title: "How to download data from NIH NDA"
categories: Linux基础
tags:  总结笔记 
keywords: NDAR Download Manager5.1 orcle SQL Developer, AWS 
author: xiaoxiao
description: 
---

* content
{:toc}

I am going to download  paper data, which is deposited to the NIH NIMH Data Archives (https://data-archive.nimh.nih.gov) under collection ID # 2330 and DOI:10.15154/1410419

# 1. The documents will need:

- MTA (My college' agreement)
- NDA Agreement
- NIMH account, complete a data access request

# 2. Log up NIMH account

- login NIMH account, finish  online Data Access Request for everyone in request form.
- waiting for the approved email for access to NDA shared data in: RDoCdb from NDA Help Desk.

# 3. how to download data

## 3.1 creating a package
- step1. loggin in to NDA.
- step2: form "data from paper" find the paper.
- step3. clink the download button on the left of the paper.
- step4. I get the filter data information
- step5: create a package

## 3.2  launch the download manager
- step1: choose Launch Donwload Manager
- step2: run command: 
> java -jar downloadmanager.jar -u xiaoxiaoh 109224
- The Download Manager will not allow downloads of packages larger than 5TB, we will get error information: unable to access data manager - https://ndar.nih.gov/DataManager/dataManager?wsdl

## 3.3  launch the download manager or launch a miNDAR.
- step1: choose Launch miNDAR 
- step2: install Oracle SQL Developer (Linux)
> unzip sqldeveloper-17.4.0.355.2349-no-jre.zip
> cd sqldeveloper
> ./sqldeveloper.sh
- step3: new connection
![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/Oracle_SQL_Developer_conntection.png) 
- step4: S3_LINKS -> Export -> get the exprot.sql file

## 3.4 Generate AWS Credentials
- step1: open NDAR Download Manager 5.1 
- step2: click the button of "Tools" on the top
- step3: Generate AWS Credentials(including Access Key, Secret Key, Session Token and Expiration Date)

## 3.5 install AWS Command Line Interface
> - sudo  pip install awscli
> - aws configure (input AWS Access Key ID, AWS Secret Access Key and Defualt region name: us-east-1 )
> - add aws_session_token to the ~/.aws/credentials
> - [xhao@EG02 2018-clone-diff]$ cat ~/.aws/credentials
> - [default]
> - aws_access_key_id = ASIAJMIVXTOPDQXUI3SA
> - aws_secret_access_key = Hz9gTd6pkO09f3tQGi3mK9pBl/uwIKdiFL3O6sBJ
> - aws_session_token=FQoDYXdzEB0aDEsQOMHkz4oVAZOfcCLWAeT2hl7YhE26eZq8
> - F18VUIYQJ1ubpWLUTWSLxNTJrDetD4GQV2GMCN2Xd20dG
> - aws s3 cp s3://NDAR_Central_4/submission_13827/capture/320-FR-CX.bam . --region us-east-1

- so far, we can download data suceessfully.

# 4. Large dataset

- I have a 14.3T dataset(195 files totally) to download, so I am going to use script to run it.
> - [xhao@EG02 2018-clone-diff]$ head -n 6 sql.txt 
> - s3://NDAR_Central_4/submission_13827/capture/275-PA-clone3.bam
> - s3://NDAR_Central_4/submission_13827/capture/320-FR-CX.bam
> - s3://NDAR_Central_4/submission_13827/data/320-BG-clone4.bam
> - s3://NDAR_Central_4/submission_13827/data/320-BG-clone8.bam
> - s3://NDAR_Central_4/submission_13827/data/320-BG-clone15.bam
> - s3://NDAR_Central_4/submission_13827/data/275-FR-clone11-hx.bam

![image](https://github.com/xiaoxiaoh16/xiaoxiaoh16.github.io/raw/master/_drafts/pic/NDA_AWS_download.sh.png) 

# 5. 文件校验
因为下载的文件很多，还在下载中，无外乎用md5等算法进行所谓的校验

# 备注
