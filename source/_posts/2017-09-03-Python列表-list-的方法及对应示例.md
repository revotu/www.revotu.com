---
title: 全面剖析Python列表(list)的各方法及各操作的内部实现
date: 2017-09-03 12:45:42
tags: [Python]
permalink: python-list-methods-and-examples
---
## 构造方法 ##
方法：
```python
class list(object):
    """
    list() -> new empty list
    list(iterable) -> new list initialized from iterable's items
    """
```
除了字面量的语法`[]`构造列表，也可以用`list()`创建空列表，`list(iterable)`由可迭代对象初始化一个列表。
<!-- more -->
示例：
```python
>>> L = []
>>> L = list()

>>> L = [1, 2, 3]
>>> L = list({1, 2, 3})
>>> L = list((1, 2, 3)) 
```
## 向列表尾部添加元素 ##
方法：
```python
    def append(self, p_object): 
        """ L.append(object) -> None -- append object to end """
        pass
```
示例：
```python
>>> L = []
>>> L.append(1)
>>> L
[1]
>>> L.append((2, 3))
>>> L
[1, (2, 3)]
>>> L.append({4, 5, 6})
>>> L
[1, (2, 3), {4, 5, 6}]
>>> 
```
## 清空列表(删除列表中所有元素) ##
方法：
```python
    def clear(self):
        """ L.clear() -> None -- remove all items from L """
        pass
```
示例：
```python
>>> L = [1, 2, 3]
>>> L.clear()
>>> L
[]
```
## 列表的浅复制 ##
方法：
```python
    def copy(self):
        """ L.copy() -> list -- a shallow copy of L """
        return []
```
示例：
```python
>>> L = [1, 2, 3]
>>> L.copy()
[1, 2, 3]
>>> 
```
## 列表中某个值出现的次数 ##
方法：
```python
    def count(self, value):
        """ L.count(value) -> integer -- return number of occurrences of value """
        return 0
```
示例：
```python
>>> L = [1, 2, 3, 2, 4, 1, 1]
>>> L.count(1)
3
>>> L.count(5)
0
```
## 用迭代器中元素扩展列表 ##
方法：
```python
    def extend(self, iterable):
        """ L.extend(iterable) -> None -- extend list by appending elements from the iterable """
        pass
```
示例：
```python
>>> L = [1, 2, 3]
>>> L.extend({4, 5})
>>> L
[1, 2, 3, 4, 5]
>>> L.extend([6, 7, 8])
>>> L
[1, 2, 3, 4, 5, 6, 7, 8]
>>> 
```
## 查找某个值在列表中第一次出现的索引 ##
方法：
```python
    def index(self, value, start=None, stop=None):
        """
        L.index(value, [start, [stop]]) -> integer -- return first index of value.
        Raises ValueError if the value is not present.
        """
        return 0
```
示例：
```python
>>> L = [1, 2, 3, 2, 4, 1, 1]
>>> L.index(2)
1
>>> L.index(5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: 5 is not in list
>>> 
```
## 向列表中指定索引前面的位置插入元素 ##
方法：
```python
    def insert(self, index, p_object):
        """ L.insert(index, object) -- insert object before index """
        pass
```
示例：
```python
>>> L = [1, 2, 3]
>>> L.insert(0, 0)
>>> L
[0, 1, 2, 3]
>>> L.insert(2, 1.5)
>>> L
[0, 1, 1.5, 2, 3]
>>> 
```
## 删除并返回指定索引处的值 ##
方法：
```python
    def pop(self, index=None):
        """
        L.pop([index]) -> item -- remove and return item at index (default last).
        Raises IndexError if list is empty or index is out of range.
        """
        pass
```
示例：
```python
>>> L = ['A', 'B', 'C', 'D', 'E']
>>> L.pop()
'E'
>>> L
['A', 'B', 'C', 'D']
>>> L.pop(2)
'C'
>>> L
['A', 'B', 'D']
>>> L.pop(3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: pop index out of range
>>> 
```
## 删除列表中第一次出现的指定值 ##
方法：
```python
    def remove(self, value):
        """
        L.remove(value) -> None -- remove first occurrence of value.
        Raises ValueError if the value is not present.
        """
        pass
```
示例：
```python
>>> L = [1, 2, 3, 2, 1]
>>> L.remove(2)
>>> L
[1, 3, 2, 1]
>>> L.remove(4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
>>> 
```
## 原地反转列表(列表中所有元素反序) ##
方法：
```python
    def reverse(self):
        """ L.reverse() -- reverse *IN PLACE* """
        pass
```
示例：
```python
>>> L = [1, 2, 3]
>>> L.reverse()
>>> L
[3, 2, 1]
>>> 
```
## 原地排序列表 ##
方法：
```python
    def sort(self, key=None, reverse=False):
        """ L.sort(key=None, reverse=False) -> None -- stable sort *IN PLACE* """
        pass
```
示例：
```python
>>> L = [3, 1, 2, 5, 4]
>>> L.sort()
>>> L
[1, 2, 3, 4, 5]
>>> L.sort(reverse=True)
>>> L
[5, 4, 3, 2, 1]
>>> L = [{'x': 1, 'y': 2}, {'x': 2, 'y': 1}, {'x': 1, 'y': 3}]
>>> L.sort(key=lambda d : (d['x'], d['y']))
>>> L
[{'y': 2, 'x': 1}, {'y': 3, 'x': 1}, {'y': 1, 'x': 2}]
>>>
```
## 列表对外接口的内部实现 ##
### 列表相加 ###
内部实现方法：
```python
    def __add__(self, *args, **kwargs):
        """ Return self+value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L2 = [4, 5, 6]
>>> L3 = L1 + L2
>>> L3
[1, 2, 3, 4, 5, 6]
>>> 
```
### in语句检测值列表中是否含有某值 ###
内部实现方法：
```python
    def __contains__(self, *args, **kwargs):
        """ Return key in self. """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> 1 in L
True
>>> 4 in L
False
>>> 
```
### del语句删除指定索引处的值 ###
内部实现方法：
```python
    def __delitem__(self, *args, **kwargs):
        """ Delete self[key]. """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> del L[1]
>>> L
[1, 3]
>>> 
```
### ==比较运算符判断两个列表是否相等 ###
内部实现方法：
```python
    def __eq__(self, *args, **kwargs):
        """ Return self==value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L2 = list((1, 2, 3))
>>> L1 == L2
True
>>> L3 = [2, 1, 3]
>>> L1 == L3
False
>>> 
```
### getattr()得到list对象的指定属性 ###
内部实现方法：
```python
    def __getattribute__(self, *args, **kwargs):
        """ Return getattr(self, name). """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> getattr(L, '__hash__')
>>> getattr(L, 'a')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'a'
>>> 
```
### 通过索引下标的方式访问元素 ###
内部实现方法：
```python
    def __getitem__(self, y):
        """ x.__getitem__(y) <==> x[y] """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> L[1]
2
>>> L[0]
1
>>> 
```
### >=比较运算符比较列表 ###
内部实现方法：
```python
    def __ge__(self, *args, **kwargs):
        """ Return self>=value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L2 = [2, 0, 1]
>>> L2 >= L1
True
>>> 
```
### >比较运算符比较列表 ###
内部实现方法：
```python
    def __gt__(self, *args, **kwargs):
        """ Return self>value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L2 = [2, 0, 1]
>>> L2 > L1
True
>>> 
```
### +=增量运算符增加列表元素 ###
内部实现方法：
```python
    def __iadd__(self, *args, **kwargs):
        """ Implement self+=value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L1 += [4, 5, 6]
>>> L1
[1, 2, 3, 4, 5, 6]
>>> 
```
### *=增量运算符增加列表元素 ###
内部实现方法：
```python
    def __imul__(self, *args, **kwargs):
        """ Implement self*=value. """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> L *= 3
>>> L
[1, 2, 3, 1, 2, 3, 1, 2, 3]
>>> 
```
### list()类构造函数始化列表 ###
内部实现方法：
```python
    def __init__(self, seq=()):
        """
        list() -> new empty list
        list(iterable) -> new list initialized from iterable's items
        """
        pass
```
对外接口示例：
```python
>>> L1 = list()
>>> L1
[]
>>> L2 = list([1, 2, 3])
>>> L2
[1, 2, 3]
>>> L3 = list({1, 2, 3})
>>> L3
[1, 2, 3]
>>> 
```
### 实现iter()函数的接口 ###
内部实现方法：
```python
    def __iter__(self, *args, **kwargs):
        """ Implement iter(self). """
        pas
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> iter(L)
<list_iterator object at 0x7fbe4cbe5080>
>>> 
```
### len()函数返回列表长度 ###
内部实现方法：
```python
    def __len__(self, *args, **kwargs):
        """ Return len(self). """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> len(L)
3
>>> 
```
### <=比较运算符比较列表 ###
内部实现方法：
```python
    def __le__(self, *args, **kwargs):
        """ Return self<=value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L2 = [2, 1, 3]
>>> L1 <= L2
True
>>> 
```
### <比较运算符比较列表 ###
内部实现方法：
```python
    def __lt__(self, *args, **kwargs):
        """ Return self<value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L2 = [2, 1, 3]
>>> L1 < L2
True
>>> 
```
### *运算符重复列表元素多少次 ###
内部实现方法：
```python
    def __mul__(self, *args, **kwargs):
        """ Return self*value.n """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> L = L * 3
>>> L
[1, 2, 3, 1, 2, 3, 1, 2, 3]
>>> 
```
### !=比较运算符比较列表 ###
内部实现方法：
```python
    def __ne__(self, *args, **kwargs):
        """ Return self!=value. """
        pass
```
对外接口示例：
```python
>>> L1 = [1, 2, 3]
>>> L2 = [2, 1, 3]
>>> L1 != L2
True
>>> 
```
### repr()返回列表对象字符串表示形式 ###
内部实现方法：
```python
    def __repr__(self, *args, **kwargs):
        """ Return repr(self). """
        pass
```
对外接口示例：
```python
>>> L = [1, 2, 3]
>>> repr(L)
'[1, 2, 3]'
>>> 
```
### 通过索引下标给元素赋值 ###
内部实现方法：
```python
    def __setitem__(self, *args, **kwargs):
        """ Set self[key] to value. """
        pass
```
对外接口示例：
```python
>>> L = [1, 3, 3]
>>> L[1] = 2
>>> L
[1, 2, 3]
>>> 
```