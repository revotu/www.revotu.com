---
title: Python解析Json时注意字符串单双引号问题
date: 2017-02-14 16:13:42
tags: [Python,Json]
permalink: python-parses-json-note-the-string-single-and-double-quotation-marks
---
> Python中的字符串用单双引号标识都可以，但要注意Json中的字符串只能是双引号。

## json中字符串用双引号，切记切记！ ##
```python
import json

data_double_quote = '{"name":"revotu","age":"24"}'
data_single_quote = "{'name':'revotu','age':'24'}"

print json.loads(data_double_quote)
# dict数据 {u'age': u'24', u'name': u'revotu'}

print json.loads(data_single_quote)
# 报错 : ValueError: Expecting property name
```
<!-- more -->
python中的`json.dump()`和`json.dumps()`生成的都是规范的json数据，即都是用双引号标识的字符串。
```python
import json

data_dict = {'name':'revotu','age':'24'}

print json.dumps(data_dict)
# json数据 {"age": "24", "name": "revotu"}
```
总结 : 处理json数据时要清楚，json中的字符串是双引号标识的。所以存储传输处理json数据时都要符合json语法规范，用双引号标识json字符串。

json官方文档 : [http://www.json.org/json-zh.html](http://www.json.org/json-zh.html)