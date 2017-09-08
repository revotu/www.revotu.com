---
title: 详解Python3中range()用法
date: 2017-09-08 09:47:17
tags: [Python]
permalink: python3-range-usage
---
在Python3中，range实际上是不可改变的序列类型，返回的range对象是迭代器，参数只能为整数，通常用在for循环中循环指定的次数。有两种构造方法：
> class range(stop)
> class range(start, stop[, step])

<!-- more -->

参数只能为整数，start默认值是0，step默认值是1，如果step为0，则抛出 `ValueError`异常。
> 当step为正数时，range对象r内容是这么确定的：`r[i] = start + step*i` 并且 `i >= 0` 和 `r[i] < stop`
> 当step为负数时，range对象r内容是这么确定的：`r[i] = start + step*i` 并且 `i >= 0` 和 `r[i] > stop`
> 如果r[0]的值不满足上述约束条件，则range对象将是空的。

只有一个参数stop的情况（第一种构造方法）：
```python
>>> range(5)
range(0, 5)
>>> for i in range(5):
...     print(i)
... 
0
1
2
3
4
>>> list(range(5))
[0, 1, 2, 3, 4]
>>> list(range(0))
[]
>>> 
```
有两个参数或三个参数的情况（第二种构造方法）：
```python
>>> for i in range(1, 5):
...     print(i)
... 
1
2
3
4
>>> list(range(0, 30, 5))
[0, 5, 10, 15, 20, 25]
>>> list(range(0, 10, 3))
[0, 3, 6, 9]
>>> list(range(0, -10, -1))
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
>>> list(range(1, 0))
[]
>>> 
```
相比list和tuple，range对象总会占用固定的内存，不管range对象代表的是多大数量的元素（因为其只需存储start、stop、step三个值，按需生成每一个元素或是子序列）。

range对象实现了序列的基本操作，比如，成员检测，索引查找，切片及支持负数索引（但不支持拼接+和重复*操作，原因就是range对象中的值要满足特定模式）。
```python
>>> r = range(0, 20, 2)
>>> r
range(0, 20, 2)
>>> 11 in r
False
>>> 10 in r
True
>>> r.index(10)
5
>>> r[5]
10
>>> r[:5]
range(0, 10, 2)
>>> r[-1]
18
>>> 
```
range对象作为序列，比较是否相等使用`==`和`!=`，即比较的是序列的值而不是标识（is 和 is not）。注意，两个range对象比较相等可能会含有不同的start、stop、step属性：
```python
>>> range(0) == range(2, 1, 3)
True
>>> range(0, 3, 2) == range(0, 4, 2)
True
>>> 
```