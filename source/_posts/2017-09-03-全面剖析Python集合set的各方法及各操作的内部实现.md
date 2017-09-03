---
title: 全面剖析Python集合set的各方法及各操作的内部实现
date: 2017-09-03 21:52:11
tags: [Python]
permalink: python-set-methods-and-examples
---
## 构造方法 ##
```python
class set(object):
    """
    set() -> new empty set object
    set(iterable) -> new set object
    
    Build an unordered collection of unique elements.
    """
```
除了用字面量语法`{1, 2, 3}`创建集合，还可以用`set()`创建空集合，`set(iterable)`由可迭代对象创建集合。注意一点，创建空集合只能用`set()`，并没有相应字面量语法创建空集合（空列表`[]`，空元组`()`，空字典`{}`）。
<!-- more -->
```python
>>> s = set()
>>> s
set()
>>> s = {1, 2, 3}
>>> s
{1, 2, 3}
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
>>> s = set('ABCDE')
>>> s
{'C', 'B', 'D', 'A', 'E'}
>>> 
```
## 向集合中添加元素 ##
方法：
```python
    def add(self, *args, **kwargs):
        """
        Add an element to a set.
        
        This has no effect if the element is already present.
        """
        pass
```
示例：
```python
>>> s = set(['A', 'B', 'C'])
>>> s.add('D')
>>> s
{'C', 'B', 'D', 'A'}
>>> s = set(['A', 'B', 'C'])
>>> s.add('A')
>>> s
{'C', 'B', 'D', 'A'}
>>> 
```
## 清空集合(删除集合中所有元素) ##
方法：
```python
    def clear(self, *args, **kwargs):
        """ Remove all elements from this set. """
        pass
```
示例：
```python
>>> s = set(['A', 'B', 'C'])
>>> s.clear()
>>> s
set()
>>> 
```
## 集合的浅复制 ##
方法：
```python
    def copy(self, *args, **kwargs):
        """ Return a shallow copy of a set. """
        pass
```
示例：
```python
>>> s = set(['A', 'B', 'C'])
>>> s.copy()
{'C', 'B', 'A'}
>>> 
```
## 差集（在一个集合，而不再另一个集合） ##
方法：
```python
    def difference(self, *args, **kwargs):
        """
        Return the difference of two or more sets as a new set.
        
        (i.e. all elements that are in this set but not the others.)
        """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.difference(y)
{'A'}
>>> y.difference(x)
{'D'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
## 用差集更新左侧集合 ##
方法：
```python
    def difference_update(self, *args, **kwargs):
        """ Remove all elements of another set from this set. """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.difference_update(y)
>>> x
{'A'}
>>> y
{'C', 'B', 'D'}
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> y.difference_update(x)
>>> y
{'D'}
>>> x
{'C', 'B', 'A'}
>>> 
```
## 如果元素存在则删除 ##
方法：
```python
    def discard(self, *args, **kwargs):
        """
        Remove an element from a set if it is a member.
        
        If the element is not a member, do nothing.
        """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> x.discard('A')
>>> x
{'C', 'B'}
>>> x.discard('D')
>>> x
{'C', 'B'}
>>> 
```
## 交集（两个集合共有的元素集合）#
方法：
```python
    def intersection(self, *args, **kwargs):
        """
        Return the intersection of two sets as a new set.
        
        (i.e. all elements that are in both sets.)
        """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.intersection(y)
{'C', 'B'}
>>> y.intersection(x)
{'C', 'B'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
## 用交集更新左侧集合 ##
方法：
```python
    def intersection_update(self, *args, **kwargs):
        """ Update a set with the intersection of itself and another. """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.intersection_update(y)
>>> x
{'C', 'B'}
>>> y
{'C', 'B', 'D'}
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> y.intersection_update(x)
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B'}
>>> 
```
## 判断两个集合是否有交集（没有则返回True）  ##
方法：
```python
    def isdisjoint(self, *args, **kwargs):
        """ Return True if two sets have a null intersection. """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.isdisjoint(y)
False
>>> x = set(['A', 'B'])
>>> y = set(['C', 'D'])
>>> x.isdisjoint(y)
True
>>> 
```
## 判断集合是否是另一个集合子集 ##
方法：
```python
    def issubset(self, *args, **kwargs):
        """ Report whether another set contains this set. """
        pass
```
示例：
```python
>>> x = set(['A'])
>>> y = set(['A', 'B', 'C'])
>>> x.issubset(y)
True
>>> 
```
## 判断集合是否是另一集合父集 ##
方法：
```python
    def issuperset(self, *args, **kwargs):
        """ Report whether this set contains another set. """
        pass
```
示例：
```python
>>> x = set(['A'])
>>> y = set(['A', 'B', 'C'])
>>> y.issuperset(x)
True
>>> 
```
## 从集合中随机删除一个元素并返回 ##
方法：
```python
    def pop(self, *args, **kwargs):
        """
        Remove and return an arbitrary set element.
        Raises KeyError if the set is empty.
        """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> x.pop()
'C'
>>> x.pop()
'B'
>>> x.pop()
'A'
>>> x.pop()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'pop from an empty set'
>>> 
```
## 从集合中删除指定元素 ##
方法：
```python
    def remove(self, *args, **kwargs):
        """
        Remove an element from a set; it must be a member.
        
        If the element is not a member, raise a KeyError.
        """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> x.remove('B')
>>> x
{'C', 'A'}
>>> x.remove('D')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'D'
>>> 
```
## 对称差集（各自独自含有的元素集合）##
方法：
```python
    def symmetric_difference(self, *args, **kwargs):
        """
        Return the symmetric difference of two sets as a new set.
        
        (i.e. all elements that are in exactly one of the sets.)
        """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.symmetric_difference(y)
{'D', 'A'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
## 用对称差集更新左侧集合 ##
方法：
```python
    def symmetric_difference_update(self, *args, **kwargs):
        """ Update a set with the symmetric difference of itself and another. """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.symmetric_difference_update(y)
>>> x
{'D', 'A'}
>>> y
{'C', 'B', 'D'}
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> y.symmetric_difference_update(x)
>>> y
{'D', 'A'}
>>> x
{'C', 'B', 'A'}
>>> 
```
## 并集（两个集合所有元素去重后的集合） ##
方法：
```python
    def union(self, *args, **kwargs):
        """
        Return the union of sets as a new set.
        
        (i.e. all elements that are in either set.)
        """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.union(y)
{'B', 'D', 'A', 'C'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
## 用并集更新左侧集合 ##
方法：
```python
    def update(self, *args, **kwargs):
        """ Update a set with the union of itself and others. """
        pass
```
示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x.update(y)
>>> x
{'B', 'D', 'A', 'C'}
>>> y
{'C', 'B', 'D'}
>>> 
```
## 集合对外接口的内部实现 ##
### &运算符求交集（正向和反向）###
内部实现方法：
```python
    def __and__(self, *args, **kwargs):
        """ Return self&value. """
        pass
    def __rand__(self, *args, **kwargs):
        """ Return value&self. """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x & y
{'C', 'B'}
>>> y & x
{'C', 'B'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
### &=用交集更新左侧集合 ###
内部实现方法：
```python
    def __iand__(self, *args, **kwargs):
        """ Return self&=value. """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x &= y
>>> x
{'C', 'B'}
>>> y
{'C', 'B', 'D'}
>>> 
```
### |运算符求并集（正向和反向）  ###
内部实现方法：
```python
    def __or__(self, *args, **kwargs):
        """ Return self|value. """
        pass
    def __ror__(self, *args, **kwargs):
        """ Return value|self. """
        pass     
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x | y
{'B', 'D', 'A', 'C'}
>>> y | x
{'B', 'D', 'A', 'C'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
### |=用并集更新左侧集合 ###
内部实现方法：
```python
    def __ior__(self, *args, **kwargs):
        """ Return self|=value. """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x |= y
>>> x
{'B', 'D', 'A', 'C'}
>>> y
{'C', 'B', 'D'}
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> y |= x
>>> x
{'C', 'B', 'A'}
>>> y
{'B', 'D', 'A', 'C'}
>>> 
```
### -运算符求差集 ###
内部实现方法：
```python
    def __sub__(self, *args, **kwargs):
        """ Return self-value. """
        pass
    def __rsub__(self, *args, **kwargs):
        """ Return value-self. """
        pass        
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x - y
{'A'}
>>> y - x
{'D'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
### -=用差集更新左侧集合 ###
内部实现方法：
```python
    def __isub__(self, *args, **kwargs):
        """ Return self-=value. """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x -= y
>>> x
{'A'}
>>> y
{'C', 'B', 'D'}
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> y -= x
>>> y
{'D'}
>>> x
{'C', 'B', 'A'}
>>> 
```
### ^运算符求对称差集（正向和反向） ###
内部实现方法：
```python
    def __xor__(self, *args, **kwargs):
        """ Return self^value. """
        pass
    def __rxor__(self, *args, **kwargs):
        """ Return value^self. """
        pass        
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x ^ y
{'D', 'A'}
>>> y ^ x
{'D', 'A'}
>>> x
{'C', 'B', 'A'}
>>> y
{'C', 'B', 'D'}
>>> 
```
### ^=用对称差集更新左侧集合 ###
内部实现方法：
```python
    def __ixor__(self, *args, **kwargs):
        """ Return self^=value. """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x ^= y
>>> x
{'D', 'A'}
>>> y
{'C', 'B', 'D'}
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> y ^= x
>>> x
{'C', 'B', 'A'}
>>> y
{'D', 'A'}
>>> 
```
### 用比较运算符比较集合 ###
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
>>> x = set(['A', 'B', 'C'])
>>> y = set(['B', 'C', 'D'])
>>> x == y
False
>>> x != y
True
>>> x >= y
False
>>> x > y
False
>>> x <= y
False
>>> x < y
False
>>> 
```
### in语句检测元素是否在集合中 ###
内部实现方法：
```python
    def __contains__(self, y):
        """ x.__contains__(y) <==> y in x. """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> 'A' in x
True
>>> 'D' in x
False
>>> 
```
### len()函数得到集合长度 ###
内部实现方法：
```python
    def __len__(self, *args, **kwargs):
        """ Return len(self). """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> len(x)
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
>>> x = set(['A', 'B', 'C'])
>>> iter(x)
<set_iterator object at 0x7fa53bb73870>
>>> 
```
### getattr()得到集合对象属性 ###
内部实现方法：
```python
    def __getattribute__(self, *args, **kwargs):
        """ Return getattr(self, name). """
        pass
```
对外接口示例：
```python
>>> x = set(['A', 'B', 'C'])
>>> dir(x)
['__and__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__iand__', '__init__', '__ior__', '__isub__', '__iter__', '__ixor__', '__le__', '__len__', '__lt__', '__ne__', '__new__', '__or__', '__rand__', '__reduce__', '__reduce_ex__', '__repr__', '__ror__', '__rsub__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__xor__', 'add', 'clear', 'copy', 'difference', 'difference_update', 'discard', 'intersection', 'intersection_update', 'isdisjoint', 'issubset', 'issuperset', 'pop', 'remove', 'symmetric_difference', 'symmetric_difference_update', 'union', 'update']
>>> getattr(x, '__and__')
<method-wrapper '__and__' of set object at 0x7fa53bb60748>
>>> getattr(x, 'update')
<built-in method update of set object at 0x7fa53bb60748>
>>> 
```
### set()构造字典 ###
内部实现方法：
```python
    @staticmethod
    def __new__(*args, **kwargs): # real signature unknown
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass
    def __init__(self, seq=()): 
        """
        set() -> new empty set object
        set(iterable) -> new set object
        
        Build an unordered collection of unique elements.
        # (copied from class doc)
        """
        pass        
```
对外接口示例：
```python
>>> x = set()
>>> x 
set()
>>> x = set('ABC')
>>> x
{'C', 'B', 'A'}
>>> x = set(['A', 'B', 'C'])
>>> x
{'C', 'B', 'A'}
>>> 
```