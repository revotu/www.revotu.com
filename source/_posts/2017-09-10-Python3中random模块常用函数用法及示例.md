---
title: Python3中random模块常用函数用法及示例
date: 2017-09-10 11:50:36
tags: [Python]
permalink: python3-random-module-most-common-methods
---
python中`random`模块是用来生成伪随机数的，其常用函数及示例如下：
## 实数相关的函数 ##
### random.random() ###
用于生成一个0到1区间的随机浮点数n，0.0 <= n < 1.0
```python
>>> import random
>>> random.random()          # 0.0 <= n < 1.0
0.7609624449125756
>>> random.random() * 100    # 0.0 <= n < 100.0
37.96152233237278
>>> 
```
<!-- more -->
### random.uniform(a, b) ### 
用于生成一个指定范围内的随机浮点数n，如果 a <= b，则a <= n <= b，如果b < a，则b <= n <= a
```python
>>> import random
>>> random.uniform(1, 10)
2.889593257343294
>>> random.uniform(2.5, 10.0)
6.158924923931107
>>>
```
## 整数相关的函数 ##
### random.randrange ###
函数原型：
```python
random.randrange(stop)
random.randrange(start, stop[, step])
```
返回来自`range(start, stop, step)`中的一个随机元素，等价于`random.choice(range(start, stop, strp))`，但实际上并没有创建一个range对象。
```python
>>> import random
>>> random.randrange(10)        # Integer from 0 to 9 inclusive
7
>>> random.randrange(0, 101, 2) # Even integer from 0 to 100 inclusive
34
>>> 
```
### random.randint(a, b) ###
用于生成一个指定范围内的整数n，a <= n <= b，等价于`random.randrange(a, b+1)`。
```python
>>> import random
>>> random.randint(1, 20)
8
>>> random.randint(1, 20)
19
>>> 
```
## 序列相关的函数 ##
### random.choice(seq) ###
从序列中随机选取一个元素，如果序列为空，则抛出`IndexError`异常。
```python
>>> import random
>>> random.choice(['A', 'B', 'C', 'D'])
'C'
>>> random.choice('ABCD')
'A'
>>> random.choice(('A', 'B', 'C', 'D'))
'B'
>>> 
```
### random.shuffle(x[, random]) ###
原地打乱一个序列（可变序列类型如列表），如果要打乱的是一个不可变序列类型如字符串、元组等，则要用`random.sample(x, k=len(x))`创建一个新的打乱后的列表。
```python
>>> import random
>>> l = ['A', 'B', 'C', 'D']
>>> random.shuffle(l)
>>> l
['B', 'A', 'C', 'D']
>>> 
```
### random.sample(population, k) ###
从序列或集合中返回指定长度的列表，不会改变原序列或原集合。
```python
>>> import random
>>> random.sample(['A', 'B', 'C', 'D'], k=2)
['B', 'D']
>>> random.sample('ABCD', k=2)
['D', 'C']
>>> random.sample(set(['A', 'B', 'C', 'D']), k=2)
['C', 'B']
>>> 
```
## 初始化随机数生成器 ##
`random.seed(a=None, version=2)`用来作为一个种子初始化随机数生成器，如果a被省略或是为None，则用当前的系统时间作为值。
之所以说`radnom`模块是用来生成伪随机数，就是当初始化的种子相同时，则生成的随机数是相同的，同样随机序列也都是相同的。
```python
>>> import random
>>> random.seed(100)
>>> random.random()
0.1456692551041303
>>> random.random()
0.45492700451402135
>>> random.randint(1, 10)
3
>>> random.choice(['A', 'B', 'C', 'D'])
'D'
>>>
```
第二次如果用相同的种子初始化随机数生成器，则相应的结果是不变的：
```python
>>> import random
>>> random.seed(100)
>>> random.random()
0.1456692551041303
>>> random.random()
0.45492700451402135
>>> random.randint(1, 10)
3
>>> random.choice(['A', 'B', 'C', 'D'])
'D'
>>> 
```
这就是`random`模块伪随机数生成器的意思。