---
layout: post
title: "how to calculate time using date"
categories: 编程之魅
tags:  Python 笔记
keywords: calculate time
author: xiaoxiao
description: 
---

* content
{:toc}


## 1. datetime 模块

- datetime 模块为日期和时间处理提供了多种方法。支持方法的同时，还可格式化输出。此外，该模块还支持时区的处理。

- 包含四大类，date表示日期(年月日); datetime（年月日时分秒...）继承 date; time表示时间(时分秒...); timedelta 表示时间差; tzinfo 表示时区信息

- datetime.datetime

- datetime是date与time的结合体，包括date与time的所有信息(常用的时间处理就用datetime)

- 它的构造函数如下：datetime.datetime (year, month, day[ , hour[ , minute[ , second[ , microsecond[ , tzinfo] ] ] ] ] )，各参数的含义与date、time的构造函数中的一样，要注意参数值的范围。

## 2. if __name__ == '__main__'语句

- Make a script both importable and executable

- 如果我们是直接执行某个.py文件的时候，该文件中那么”__name__ == '__main__'“是True. 但是我们如果从另外一个.py文件通过import导入该文件的时候，这时__name__的值就是我们这个py文件的名字而不是__main__. 引入的if语句的作用就是  通过检查该模块的__name__属性来实现判断.py文件被使用的方式。从而方便我们代码复用，也可以测试模块。这个功能还有一个用处：调试代码的时候，在”if __name__ == '__main__'“中加入一些我们的调试代码，我们可以让外部模块调用的时候不执行我们的调试代码，但是如果我们想排查问题的时候，直接执行该模块文件，调试代码能够正常运行

## 3. 计算时间差
```
import datetime

def intervalSeconds(date1, date2):
	timedelta = date2 - date1
	interval = timedelta.days * 24 * 3600 + timedelta.seconds
	return interval

date1 = datetime.datetime(2018,01,11,16,58,16)
date2 = datetime.datetime(2018,01,11,19,8,57)

if __name__ == "__main__":
	print intervalSeconds(date1, date2)

```

## 备注
