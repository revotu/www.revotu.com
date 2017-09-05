---
title: 三种pythonic的方法交换字典的键keys和值values
date: 2017-09-05 13:32:33
tags: [Python]
permalink: three-pythonic-ways-to-swap-dict-keys-and-values
---
## 方法一：字典推导式 ##
```python
>>> d = {'A' : 1, 'B' : 2, 'C' : 3}
>>> d1 = {v:k for k,v in d.items()}
>>> d1
{1: 'A', 2: 'B', 3: 'C'}
>>> 
```
<!-- more -->
## 方法二：生成器表达式作为dict()的参数 ##
```python
>>> d = {'A' : 1, 'B' : 2, 'C' : 3}
>>> d2 = dict((v,k) for k,v in d.items())
>>> d2
{1: 'A', 2: 'B', 3: 'C'}
>>> 
```
## 方法三： zip返回的迭代器作为dict()的参数 ##
```python
>>> d = {'A' : 1, 'B' : 2, 'C' : 3}
>>> d3 = dict(zip(d.values(), d.keys()))
>>> d3
{1: 'A', 2: 'B', 3: 'C'}
>>> 
```