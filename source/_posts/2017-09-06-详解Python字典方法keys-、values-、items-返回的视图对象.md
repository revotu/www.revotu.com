---
title: 详解Python字典方法keys()、values()、items()返回的视图对象
date: 2017-09-06 22:51:02
tags: [Python]
permalink: python-dict-methods-keys-values-items-return-views
---
在Python3中`dict.keys()`、`dict.values()`、`dict.items()`返回的都是视图对象，而不在是列表。视图对象可以看做是对字典开放的一个窗口，可以动态的反应字典的变化，也就是字典所做的改动在视图对象上都动态体现出来。
<!-- more -->
视图对象同样可迭代，当迭代keys和values期间字典没有发生变化的话，keys和values是相对应的。也就是说允许像这样创建`(value, key)`对：
```python
>>> pairs = zip(d.values(), d.keys())
# OR
>>> pairs = [(v,k) for k,v in d.items()]
```
对于`dict.keys()`返回的键视图对象来说是一个类似集合的对象，因为其中键元素是唯一的且可哈希，如果所有的values也是可哈希（通常来说都是不可哈希的因为字典的值不能保证唯一性）的话，那么整个（key，value）对也就是可哈希的，此时`dict.values()`和`dict.items()`也就是类似集合的对象。对于类似集合的对象来说，其支持集合定义的基本运算，如`|`、`&`、`^`、`-`、`==`等。
举例如下：
```python
>>> d = {'A' : 1, 'B' : 2, 'C' : 3, 'D' : 4}
>>> keys = d.keys()
>>> values = d.values()

>>> # iteration
>>> for k,v in d.items():
...     print(k,v)
... 
D 4
B 2
A 1
C 3
>>>

# len and in
>>> len(keys)
4
>>> 'A' in keys
True
>>> 


>>> # keys and values are iterated over in the same order
>>> list(keys)
['D', 'B', 'A', 'C']
>>> list(values)
[4, 2, 1, 3]
>>>

>>> # view objects are dynamic and reflect dict changes
>>> del d['A']
>>> del d['B']
>>> list(keys)
['D', 'C']
>>> list(values)
[4, 3]
>>>

>>> # set operations
>>> keys & {'D', 'E', 'F'}
{'D'}
>>> keys ^ {'D', 'E', 'F'}
{'F', 'E', 'C'}
>>> 
```
