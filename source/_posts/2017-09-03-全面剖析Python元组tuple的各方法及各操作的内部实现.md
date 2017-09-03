---
title: 全面剖析Python元组tuple的各方法及各操作的内部实现
date: 2017-09-03 18:17:57
tags: [Python]
permalink: python-tuple-methods-and-examples
---
## 构造方法 ##
```python
class tuple(object):
    """
    tuple() -> empty tuple
    tuple(iterable) -> tuple initialized from iterable's items
    
    If the argument is a tuple, the return value is the same object.
    """
```
除了字面量语法`()`构造元组，还可以通过`tuple()`创建空元组，`tuple(iterable)`由可迭代对象创建元组。示例如下：
<!-- more -->
```python
>>> T = ()
>>> T
()
>>> T = tuple()
>>> T
()
>>> T = (1, 2, 3)
>>> T
(1, 2, 3)
>>> T = tuple([1, 2, 3])
>>> T
(1, 2, 3)
>>> T = tuple({1, 2, 3})
>>> T
(1, 2, 3)
>>> 
```
## 元组中某个值出现的次数 ##
方法：
```python
    def count(self, value):
        """ T.count(value) -> integer -- return number of occurrences of value """
        return 0
```
示例：
```python
>>> T = (1, 2, 3, 1, 2)
>>> T.count(2)
2
>>> 
```
## 元组中某个值第一次出现的索引 ##
方法：
```python
    def index(self, value, start=None, stop=None):
        """
        T.index(value, [start, [stop]]) -> integer -- return first index of value.
        Raises ValueError if the value is not present.
        """
        return 0
```
示例：
```python
>>> T = (1, 2, 3, 1, 2)
>>> T.index(2)
1
>>> T.index(2, 2)
4
>>> T.index(2, 2, 4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: tuple.index(x): x not in tuple
>>> 
```
## 元组对外接口的内部实现 ##
### 元组相加 ###
内部实现方法：
```python
    def __add__(self, *args, **kwargs):
        """ Return self+value. """
        pass
```
对外接口示例：
```python
>>> T1 = (1, 2, 3)
>>> T2 = (4, 5, 6)
>>> T3 = T1 + T2
>>> T3
(1, 2, 3, 4, 5, 6)
>>> 
```
### in语句检测元组中是否含有某值 ###
内部实现方法：
```python
    def __contains__(self, *args, **kwargs):
        """ Return key in self. """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> 1 in T
True
>>> 4 in T
False
>>> '1' in T
False
>>> 
```
### 通过比较运算符比较元组 ###
内部实现方法：
```python
    def __eq__(self, *args, **kwargs):
        """ Return self==value. """
        pass
    def __ne__(self, *args, **kwargs):
        """ Return self!=value. """
        pass        
    def __ge__(self, *args, **kwargs):
        """ Return self>=value. """
        pass
    def __gt__(self, *args, **kwargs):
        """ Return self>value. """
        pass   
    def __le__(self, *args, **kwargs):
        """ Return self<=value. """
        pass
    def __lt__(self, *args, **kwargs):
        """ Return self<value. """
        pass
```
对外接口示例：
```python
>>> T1 = (1, 2, 3)
>>> T2 = (2, 1, 3)
>>> T1 == T2
False
>>> T1 != T2
True
>>> T1 >= T2
False
>>> T1 > T2
False
>>> T1 <= T2
True
>>> T1 < T2
True
>>> 
```
### len()函数得到元组长度 ###
内部实现方法：
```python
    def __len__(self, *args, **kwargs):
        """ Return len(self). """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> len(T)
3
>>> 
```
### 创建可迭代对象 ###
内部实现方法：
```python
    def __iter__(self, *args, **kwargs):
        """ Implement iter(self). """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> iter(T)
<tuple_iterator object at 0x7fc3e34cc160>
>>> 
```
### 元组实现乘法运算符(左乘和右乘) ###
内部实现方法：
```python
    def __mul__(self, *args, **kwargs):
        """ Return self*value.n """
        pass
    def __rmul__(self, *args, **kwargs):
        """ Return self*value. """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> T * 3
(1, 2, 3, 1, 2, 3, 1, 2, 3)
>>> 3 * T
(1, 2, 3, 1, 2, 3, 1, 2, 3)
>>> 
```
### 元组实现增量运算符 ###
内部实现方法：
```python
    def __iadd__(self, *args, **kwargs):
        """ Implement self+=value. """
        pass

    def __imul__(self, *args, **kwargs):
        """ Implement self*=value. """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> T += (4, 5, 6)
>>> T
(1, 2, 3, 4, 5, 6)
>>> T = (1, 2, 3)
>>> T *= 3
>>> T
(1, 2, 3, 1, 2, 3, 1, 2, 3)
>>> 
```
### 通过索引访问、设置元素值 ###
内部实现方法：
```python
    def __getitem__(self, y):
        """ x.__getitem__(y) <==> x[y] """
        pass
    def __setitem__(self, *args, **kwargs):
        """ Set self[key] to value. """
        pass        
```
对外接口示例：
```python
>>> T = (1, 2, [3, 4])
>>> T[2]
[3, 4]
>>> T[2].append(5)
>>> T
(1, 2, [3, 4, 5])
>>> 
```
### repr()返回元组对象字符串表示形式 ###
内部实现方法：
```python
    def __repr__(self, *args, **kwargs):
        """ Return repr(self). """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> repr(T)
'(1, 2, 3)'
>>> 
```
### hash()返回元组对象的哈希值 ###
内部实现方法：
```python
    def __hash__(self, *args, **kwargs):
        """ Return hash(self). """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> hash(T)
2528502973977326415
>>>
```
### tuple()构造元组 ###
内部实现方法：
```python
    @staticmethod
    def __new__(*args, **kwargs):
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass
    def __init__(self, seq=()):
        """
        tuple() -> empty tuple
        tuple(iterable) -> tuple initialized from iterable's items
        
        If the argument is a tuple, the return value is the same object.
        # (copied from class doc)
        """
        pass
```
对外接口示例：
```python
>>> T = tuple()
>>> T
()
>>> T = tuple([1, 2, 3])
>>> T
(1, 2, 3)
>>> T = tuple({1, 2, 3})
>>> T
(1, 2, 3)
>>> 
```
### getattr()得到元组某属性 ###
内部实现方法：
```python
    def __getattribute__(self, *args, **kwargs):
        """ Return getattr(self, name). """
        pass
```
对外接口示例：
```python
>>> T = (1, 2, 3)
>>> dir(T)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'count', 'index']
>>> getattr(T, '__add__')
<method-wrapper '__add__' of tuple object at 0x7fc3e34c9870>
>>> getattr(T, 'index')
<built-in method index of tuple object at 0x7fc3e34c9870>
>>> 
```