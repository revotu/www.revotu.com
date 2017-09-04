---
title: Python内置函数any和all的用法及示例
date: 2017-09-05 00:14:20
tags: [Python]
permalink: python-built-in-function-any-and-all
---
## any(iterable) ##
 `any(iterable)`函数接受一个可迭代对象iterable作为参数，如果iterable中`有元素为真值`则返回True，否则返回False。如果iterable为空则返回False。
```python
>>> any([1, 2, 3])
True
>>> any([1, 0, 0.0])
True
>>> any(['', 0, 0.0])
False
>>> any([])
False
```
<!-- more -->
等价代码实现：
```python
def any(iterable):
    for item in iterable:
        if item:
            return True
    return False
```
## all(iterable) ##
`all(iterable)`函数接受一个可迭代对象iterable作为参数，如果iterable中`所有元素为真值`则返回True，否则返回False。如果iterable为空则返回True。
```python
>>> all([1, 2, 3])
True
>>> all(['A', 1, (3, 4)])
True
>>> all([1, 0, 0.0])
False
>>> all([])
True
>>> 
```
等价代码实现：
```python
def all(iterable):
    for item in iterable:
        if not item:
            return False
    return True
```
## 注意 ##
`any()`和`all()`两个函数都会短路，即一旦确定了结果就立即停止迭代，下面用个例子来说明：
```python
>>> g = (n for n in [0, 0.0, 1, 2])
>>> any(g)
True
>>> next(g)
2
>>> 
```
## 应用 ##
如果有一段文本，想检测某些字符是否在文本中（全在和存在）：
```python
>>> text = """Beautiful is better than ugly.
... Explicit is better than implicit."""
>>> any(s in text for s in 'aeiou')
True
>>> all(s in text for s in 'aeiou')
False
>>> 
```
检测密码强度是否合格（大写字母，小写字母，数字，非字母数字的字符）：
```python
def valid(pwd):
    a = any(map(str.isupper, pwd))
    b = any(map(str.islower, pwd))
    c = any(map(str.isdigit, pwd))
    d = not all(map(str.isalnum, pwd))
    return all([a, b, c, d])

print(valid('123'))          # False
print(valid('123abc'))       # False
print(valid('123abcABC'))    # False
print(valid('123abcABC@'))   # True
```