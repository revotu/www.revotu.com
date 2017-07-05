---
title: 解决Scrapy中xpath用到中文报错问题
date: 2017-05-28 18:36:42
tags: [Scrapy,Python,XPATH]
permalink: solve-unicode-erros-using-xpath-in-scrapy
---
## 问题描述 ##
```Python
links = sel.xpath('//i[contains(@title,"置顶")]/following-sibling::a/@href').extract()
```
报错：ValueError: All strings must be XML compatible: Unicode or ASCII, no NULL bytes or control characters
<!-- more -->
## 解决方法 ##
方法一：将整个xpath语句转成Unicode
```Python
links = sel.xpath(u'//i[contains(@title,"置顶")]/following-sibling::a/@href').extract()
```
方法二：xpath语句用已转成Unicode的title变量
```Python
title = u"置顶"
links = sel.xpath('//i[contains(@title,"%s")]/following-sibling::a/@href' %(title)).extract()
```
方法三：直接用xpath中变量语法(`$`符号加变量名)`$title`, 传参title即可
```Python
links = sel.xpath('//i[contains(@title,$title)]/following-sibling::a/@href', title="置顶").extract()
```
方法四：从`__future__`模块导入`unicode_literals`，使用Python3的字符串默认编码是Unicode的特性
```Python
from __future__ import unicode_literals #放在文件首行
```