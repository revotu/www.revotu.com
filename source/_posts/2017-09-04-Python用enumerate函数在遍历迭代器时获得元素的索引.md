---
title: Python用enumerate函数在遍历迭代器时获得元素的索引
date: 2017-09-04 11:44:44
tags: [Python]
permalink: get-iterable-index-with-enumerate-funtion
---
for循环可以遍历可迭代对象，得到的是每个元素的值，无法得到每个值所对应的索引下标，与内置的`range`函数结合，可以得到元素值的同时，得到其索引下标。后面将说明使用内置的`enumerate`函数才是解决这个问题的pythonic方法。
## 使用range函数遍历时得到索引 ##
```python
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
for i in range(len(seasons)):
    print(i, seasons[i])
```
输出结果：
```python
0 Spring
1 Summer
2 Fall
3 Winter
```
但是，上述代码有些生硬，我们必须获取列表长度，并且通过下标的方式访问每个元素，这种代码不便于理解。
<!-- more -->
## 使用enumerate函数遍历时得到索引 ##
Python提供了内置`enumerate`函数，以解决遍历时同时要得到对应元素的索引这个问题。
```python
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
for i, season in enumerate(seasons):
    print(i , season)
```
输出结果：
```python
0 Spring
1 Summer
2 Fall
3 Winter
```
很明显，使用`enumerate`函数遍历迭代器时同时得到每个元素的索引是更加pythonic的方式。
## enumerate函数原理 ##
enumerate函数本质是一个生成器函数，可以把各种迭代器（也包括序列及各种支持迭代的对象）包装为生成器，以便稍后产生输出值。生成器每次产生一对输出值，值为元组：
```python
(0, seq[0]), (1, seq[1]), (2, seq[2]), ...
```
其中，前者表示循环下标，后者表示从迭代器中获取到的对应元素。示例：
```python
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> enumerate(seasons)
<enumerate object at 0x7fe606b51090>
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
>>> 
```
## enumerate函数指定第二个参数（初始值） ##
可以给enumerate函数指定第二个参数，以指定开始计数时所用到的值（默认是0）。
```python
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
for i, season in enumerate(seasons, start=1):
    print(i , season)
```
输出结果：
```python
1 Spring
2 Summer
3 Fall
4 Winter
```
## enumerate函数等价实现 ##
enumerate函数等价于如下实现代码：
```python
def enumerate(iterable, start=0):
    for item in iterable:
        yield start, item
        start +=1
```