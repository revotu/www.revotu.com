---
title: Python2和Python3中字典的keys()、values()、items()方法的区别
date: 2017-09-05 15:13:40
tags: [Python]
permalink: difference-dict-keys-values-items-between-python2-and-python3
---
## 区别 ##
Python2字典有三个版本的keys、values、items方法。分别是：
> 列表版本：`keys()`、`values()`、`items()`
> 迭代器版本：`iterkeys()`、`itervalues()`、`iteritems()`
> 视图版本：`viewkeys()`、`viewvalues()`、`viewitems()`

Python3字典只有一个版本的keys、values、items方法。
> 视图版本：`keys()`、`values()`、`items()`

其他Python2中的iter系列和view系列方法都不存在。
<!-- more -->
## 说明 ##
Python2中列表版本返回的是列表，可以实际存储结果，占用额外内存。
```python
>>> d = {'A' : 1, 'B' : 2, 'C' : 3}
>>> d.keys()
['A', 'C', 'B']
>>> d.values()
[1, 3, 2]
>>> d.items()
[('A', 1), ('C', 3), ('B', 2)]
>>> 
```
Python2中迭代器版本返回的是迭代器，不占用额外内存，按需生成元素。
```python
>>> d = {'A' : 1, 'B' : 2, 'C' : 3}
>>> d.iterkeys()
<dictionary-keyiterator object at 0x7f12c682e7e0>
>>> list(d.itervalues())
[1, 3, 2]
>>> for k,v in d.iteritems():
...     print(k,v)
... 
('A', 1)
('C', 3)
('B', 2)
>>> 
```
Python2视图版本返回的是视图对象，作为一个窗口能动态反应字典的变化，当然也可以迭代。
```python
>>> d = {'A' : 1, 'B' : 2, 'C' : 3}
>>> keys = d.viewkeys()
>>> keys
dict_keys(['A', 'C', 'B'])
>>> d['D'] = 4
>>> keys
dict_keys(['A', 'C', 'B', 'D'])
>>> del d['A']
>>> keys
dict_keys(['C', 'B', 'D'])
>>> d
{'C': 3, 'B': 2, 'D': 4}
>>> list(d.viewvalues())
[3, 2, 4]
>>> for k,v in d.viewitems():
...     print(k,v)
... 
('C', 3)
('B', 2)
('D', 4)
>>> 
```
Python3字典只有`keys()`、`values()`、`items()`这一个视图版本，相当于Python2的view系列方法。