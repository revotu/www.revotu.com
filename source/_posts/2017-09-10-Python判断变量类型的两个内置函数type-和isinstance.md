---
title: Python判断变量类型的两个内置函数type()和isinstance()
date: 2017-09-10 23:05:49
tags: [Python]
permalink: python-check-variable-type
---
Python中判断对象类型可以用内置函数`type()`：
```python
>>> type([]) is list
True
>>> type({}) is dict
True
>>> type('') is str
True
>>> type(0) is int
True
>>> type([])
<class 'list'>
>>> type({})
<class 'dict'>
>>> 
```
`type()`函数也可以用在自定义的类型上，但是只能得到其本身类型，无法得知其继承自什么类型。
<!-- more -->
```python
class A:
    pass

class B(A):
    pass

>>> a = A()
>>> b = B()
>>> type(a) is A
True
>>> type(b) is B
True
>>> 
```
但是，判断子类B的实例b是否是父类A类型时，则是False：
```python
>>> type(b) is A
False
>>> 
```
为了解决这个问题，需要使用`isinstance()`内置函数，用来判断对象是否是类的实例。
```python
>>> isinstance(b, A)
True
>>> isinstance(b, B)
True
>>> isinstance(a, A)
True
>>> isinstance(a, B)
False
>>> isinstance([], list)
True
>>> isinstance({}, dict)
True
>>> isinstance(0, int)
True
>>> 
```
因此，通常情况下，判断对象类型用`isinstance()`是更好的选择，相比`type()`，它能得到对象继承自什么类型。
而且，`isinstance()`函数中的类型参数还可以是元组，以便检测对象是否是多个类型中的一种。
```python
>>> isinstance([], (tuple, list, dict))
True
>>> isinstance((), (tuple, list, dict))
True
>>> isinstance({}, (tuple, list, dict))
True
>>> 
```