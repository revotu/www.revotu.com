---
title: Python之list、dict、json等常见操作
date: 2015-06-24 17:26:31
tags: [Python]
permalink: common-python-questions-of-list-dict-json
---
## 合并两个list并去除重复元素 ##
方法一：合并list用+运算符，再用set去除重复元素，最后转换回list。
```Python
>>> L1 = [1,2,3,4]
>>> L2 = [3,4,5,6]
>>> L3 = list(set(L1 + L2))
>>> L3
[1, 2, 3, 4, 5, 6]
```
<!-- more -->
方法二：用其中一个list+另一个去除在前面list后的元素组成的list。
```Python
>>> L1 = [1,2,3,4]
>>> L2 = [3,4,5,6]
>>> L3 = L1 + [x for x in L2 if x not in L1]
>>> L3
[1, 2, 3, 4, 5, 6]
```
## 合并两个list生成dict（一个为keys，一个为values） ##
先对两个list用zip函数，最后转换成dict。
```Python
>>> keys = ['a','b','c']
>>> values = [1,2,3]
>>> dictionary = dict(zip(keys,values))
>>> dictionary
{'a': 1, 'c': 3, 'b': 2}
```
## dict转换成元素为tuple的list ##
方法一：用dict的items函数。
```Python
>>> d = {'a': 1, 'c': 3, 'b': 2}
>>> d.items()
[('a', 1), ('c', 3), ('b', 2)]
```
方法二：用dict的iteritems函数。
```Python
>>> d = {'a': 1, 'c': 3, 'b': 2}
>>> [(k, v) for k, v in d.iteritems()]
[('a', 1), ('c', 3), ('b', 2)]
```
## 两个list元素分别相加生成求和后的list ##
方法一：用map和operator.add。
```Python
>>> from operator import add
>>> L1 = [1,2,3]
>>> L2 = [4,5,6]
>>> L3 = map(add, L1, L2)
>>> L3
[5, 7, 9]
```
方法二：用zip和sum函数。
```Python
>>> L1 = [1,2,3]
>>> L2 = [4,5,6]
>>> L3 = [sum(x) for x in zip(L1,L2)]
>>> L3
[5, 7, 9]
```
## json转换成dict ##
用json.loads()方法。
```Python
>>> import json
>>> json_data = '{"key 1": "value 1", "key 2": "value 2"}'
>>> dict_data = json.loads(json_data)
>>> dict_data
{u'key 1': u'value 1', u'key 2': u'value 2'}
```
## dict转换成json ##
用json.dumps()方法。
```Python
>>> import json
>>> dict_data = {"key 1": "value 1", "key 2": "value 2"}
>>> json_data = json.dumps(dict_data)
>>> json_data
'{"key 1": "value 1", "key 2": "value 2"}'
```
## json数据写入到文件 ##
用json.dump()方法。
```Python
>>> import json
>>> with open('data.txt', 'w') as outfile:
        json.dump(data, outfile)
```
## 从文件读入json数据 ##
用json.load()方法。
```Python
>>> import json
>>> with open('data.txt') as infile:
        data = json.load(infile)
```
## 字典中是否存在某个键或值 ##
用in关键字判断dict是否有某个键或值
```Python
>>> d = {'a': 1, 'c': 3, 'b': 2}
>>> 'c' in d
True
>>> 2 in d.values()
True
```
## 随机选取list中某一元素 ##
用`random.choice()`方法。
```Python
>>> import random
>>> L = ["My" ,"Python" ,"World"]
>>> random.choice(L)
'Python'
```
## 向列表中添加元素至首位 ##
用`list.insert(i, x)`方法，或是直接列表相加。
```Python
>>> a = ["v1", "v2", "v3", "v4"]
>>> print a
['v1', 'v2', 'v3', 'v4']
>>> a.insert(0, "v5")
>>> print a
['v5', 'v1', 'v2', 'v3', 'v4']
>>> a = ["v6"] + a
>>> print a
['v6', 'v5', 'v1', 'v2', 'v3', 'v4']
>>> 
```
## 判断字典中是否有重复值 ##
```Python
>>> def has_duplicates(d):
	return len(d) != len(set(d.values()))

>>> print has_duplicates({'A': 1, 'B': 1, 'C': 3})
True
>>> 
```
## 打印出字典中的重复值及有相同值的keys##
```Python
>>> dest_dict = {}
>>> orig_dict = {'A': 1, 'B': 1, 'C': 3}

>>> for k,v in orig_dict.iteritems():
	dest_dict.setdefault(v, []).append(k)

>>> print [k for k, v in dest_dict.items() if len(v) > 1]
[1]
>>> print [v for k, v in dest_dict.items() if len(v) > 1]
[['A', 'B']]
>>> 
```