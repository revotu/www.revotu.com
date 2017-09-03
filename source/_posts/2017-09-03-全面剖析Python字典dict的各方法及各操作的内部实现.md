---
title: 全面剖析Python字典dict的各方法及各操作的内部实现
date: 2017-09-03 15:25:09
tags: [Python]
permalink: python-dict-methods-and-examples
---
## 构造方法 ##
```python
class dict(object):
    """
    dict() -> new empty dictionary
    dict(mapping) -> new dictionary initialized from a mapping object's
        (key, value) pairs
    dict(iterable) -> new dictionary initialized as if via:
        d = {}
        for k, v in iterable:
            d[k] = v
    dict(**kwargs) -> new dictionary initialized with the name=value pairs
        in the keyword argument list.  For example:  dict(one=1, two=2)
    """
```
<!-- more -->
除了字面量的语法`{}`构造字典，也可以用`dict()`构造字典。参数有几种，示例如下：
```python
>>> D = {}
>>> D
{}
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D
{'A': 1, 'B': 2, 'C': 3}
>>> D = dict()
>>> D
{}
>>> D = dict({'A': 1, 'B': 2, 'C': 3})
>>> D
{'A': 1, 'B': 2, 'C': 3}
>>> D = dict([('A', 1), ('B', 2), ('C', 3)])
>>> D
{'A': 1, 'B': 2, 'C': 3}
>>> D = dict(A=1, B=2, C=3)
>>> D
{'C': 3, 'B': 2, 'A': 1}
>>> 
```
## 清空字典(删除字典中所有元素) ##
方法：
```python
    def clear(self):
        """ D.clear() -> None.  Remove all items from D. """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.clear()
>>> D
{}
>>>
```
## 字典浅复制 ##
方法：
```python
    def copy(self):
        """ D.copy() -> a shallow copy of D """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> d = D.copy()
>>> d
{'A': 1, 'B': 2, 'C': 3}
>>> 
```
## 由键组成的可迭代对象创建新字典 ##
方法：
```python
    @staticmethod
    def fromkeys(*args, **kwargs): # real signature unknown
        """ Returns a new dict with keys from iterable and values equal to value. """
        pass
```
示例：
```python
>>> dict.fromkeys({'name', 'age', 'sex'})
{'age': None, 'sex': None, 'name': None}
>>> dict.fromkeys({'name', 'age', 'sex'}, 10)
{'age': 10, 'sex': 10, 'name': 10}
>>> 
```
## 返回指定键的值，如果键不存在返回默认值 ##
方法：
```python
    def get(self, k, d=None):
        """ D.get(k[,d]) -> D[k] if k in D, else d.  d defaults to None. """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.get('A')
1
>>> D.get('D', 0)
0
>>> 
```
## 得到字典中所有元素的可迭代对象 ##
方法：
```python
    def items(self):
        """ D.items() -> a set-like object providing a view on D's items """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.items()
dict_items([('A', 1), ('B', 2), ('C', 3)])
>>> for k, v in D.items():
...     print(k, v)
... 
A 1
B 2
C 3
>>> list(D.items())
[('A', 1), ('B', 2), ('C', 3)]
>>> 
```
## 得到字典中所有键的可迭代对象 ##
方法：
```python
    def keys(self):
        """ D.keys() -> a set-like object providing a view on D's keys """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.keys()
dict_keys(['A', 'B', 'C'])
>>> for k in D.keys():
...     print(k)
... 
A
B
C
>>> list(D.keys())
['A', 'B', 'C']
>>> 
```
## 删除指定键，并返回对应的值 ##
方法：
```python
    def pop(self, k, d=None):
        """
        D.pop(k[,d]) -> v, remove specified key and return the corresponding value.
        If key is not found, d is returned if given, otherwise KeyError is raised
        """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.pop('A')
1
>>> D
{'B': 2, 'C': 3}
>>> D.pop('D', 0)
0
>>> D
{'B': 2, 'C': 3}
>>> 
```
## 删除并返回键值对所组成的元组 ##
方法：
```python
    def popitem(self):
        D.popitem() -> (k, v), remove and return some (key, value) pair as a
        2-tuple; but raise KeyError if D is empty.
        """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.popitem()
('A', 1)
>>> D.popitem()
('B', 2)
>>> D.popitem()
('C', 3)
>>> D
{}
>>> 
```
## 键存在则返回对应的值，不存在则设置值并返回值 ##
方法：
```python
    def setdefault(self, k, d=None):
        """ D.setdefault(k[,d]) -> D.get(k,d), also set D[k]=d if k not in D """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.setdefault('A', 0)
1
>>> D
{'A': 1, 'B': 2, 'C': 3}
>>> D.setdefault('D', 0)
0
>>> D
{'D': 0, 'A': 1, 'B': 2, 'C': 3}
>>> 
```
## 用另一个可迭代对象更新字典 ##
方法：
```python
    def update(self, E=None, **F):
        """
        D.update([E, ]**F) -> None.  Update D from dict/iterable E and F.
        If E is present and has a .keys() method, then does:  for k in E: D[k] = E[k]
        If E is present and lacks a .keys() method, then does:  for k, v in E: D[k] = v
        In either case, this is followed by: for k in F:  D[k] = F[k]
        """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.update({'A': 0, 'D': 1})
>>> D
{'D': 1, 'A': 0, 'B': 2, 'C': 3}
>>> D.update({'A': 0, 'D': 1}, A=1, D=4)
>>> D
{'D': 4, 'A': 1, 'B': 2, 'C': 3}
>>> 
```
## 得到字典中所有值的可迭代对象 ##
方法：
```python
    def values(self):
        """ D.values() -> an object providing a view on D's values """
        pass
```
示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D.values()
dict_values([1, 2, 3])
>>> for v in D.values():
...     print(v)
... 
1
2
3
>>> list(D.values())
[1, 2, 3]
>>> 
```
## 字典对外接口的内部实现 ##
### in语句检测字典中是否有某键 ###
内部实现方法：
```python
    def __contains__(self, *args, **kwargs):
        """ True if D has a key k, else False. """
        pass
```
对外接口示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> 'A' in D
True
>>> 'D' in D
False
>>> 
```
### len()函数返回字典长度 ###
内部实现方法：
```python
    def __len__(self, *args, **kwargs):
        """ Return len(self). """
        pass
```
对外接口示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> len(D)
3
>>> 
```
### repr()返回字典对象字符串表示形式 ###
内部实现方法：
```python
    def __repr__(self, *args, **kwargs):
        """ Return repr(self). """
        pass
```
对外接口示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> repr(D)
"{'A': 1, 'B': 2, 'C': 3}"
>>> 
```
### dict()构造字典 ###
内部实现方法：
```python
    def __init__(self, seq=None, **kwargs):
        """
        dict() -> new empty dictionary
        dict(mapping) -> new dictionary initialized from a mapping object's
            (key, value) pairs
        dict(iterable) -> new dictionary initialized as if via:
            d = {}
            for k, v in iterable:
                d[k] = v
        dict(**kwargs) -> new dictionary initialized with the name=value pairs
            in the keyword argument list.  For example:  dict(one=1, two=2)
        # (copied from class doc)
        """
        pass
 
    @staticmethod
    def __new__(*args, **kwargs): # real signature unknown
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass
```
对外接口示例：
```python
>>> D = dict()
>>> D
{}
>>> D = dict({'A': 1, 'B': 2, 'C': 3})
>>> D
{'A': 1, 'B': 2, 'C': 3}
>>> D = dict([('A', 1), ('B', 2), ('C', 3)])
>>> D
{'A': 1, 'B': 2, 'C': 3}
>>> D = dict(A=1, B=2, C=3)
>>> D
{'C': 3, 'B': 2, 'A': 1}
>>> 
```
### 通过比较运算符比较字典 ###
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
>>> D1 = {'A': 1, 'B': 2, 'C': 3}
>>> D2 = {'B': 2, 'C': 3, 'A': 1}
>>> D1 == D2
True
>>> 
```
### 通过键的方式访问、设置、删除元素 ###
内部实现方法：
```python
    def __getitem__(self, y):
        """ x.__getitem__(y) <==> x[y] """
        pass
    def __setitem__(self, *args, **kwargs):
        """ Set self[key] to value. """
        pass
    def __delitem__(self, *args, **kwargs):
        """ Delete self[key]. """
        pass
```
对外接口示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> D['A']
1
>>> D['B'] = 0
>>> D
{'A': 1, 'B': 0, 'C': 3}
>>> del D['C']
>>> D
{'A': 1, 'B': 0}
>>> 
```
### getattr()得到字典某属性 ###
内部实现方法：
```python
    def __getattribute__(self, *args, **kwargs):
        """ Return getattr(self, name). """
        pass
```
对外接口示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> getattr(D, '__hash__')
>>> getattr(D, 'sss')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'dict' object has no attribute 'sss'
>>> 
```
### 实现iter()函数的接口  ###
内部实现方法：
```python
    def __iter__(self, *args, **kwargs):
        """ Implement iter(self). """
        pass
```
对外接口示例：
```python
>>> D = {'A': 1, 'B': 2, 'C': 3}
>>> iter(D)
<dict_keyiterator object at 0x7f0f1f0f9e08>
>>>
```