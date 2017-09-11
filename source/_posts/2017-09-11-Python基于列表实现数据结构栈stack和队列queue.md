---
title: Python基于列表实现数据结构栈stack和队列queue
date: 2017-09-11 14:14:37
tags: [Python]
permalink: python-implement-data-stucture-stack-and-queue-base-on-list
---
## python中栈的实现 ##
栈是一种后进先出（LIFO）的数据结构，只能在一端（栈顶）插入和删除元素，而python中的列表的`append()`方法对应的就是向栈顶添加元素，列表的`pop()`方法对应的就是弹出栈顶元素，因此，python中的列表可以作为栈这种数据结构。
```python
>>> stack = [3, 4, 5]
>>> stack.append(6)
>>> stack.append(7)
>>> stack
[3, 4, 5, 6, 7]
>>> stack.pop()
7
>>> stack.pop()
6
>>> stack.pop()
5
>>> stack
[3, 4]
>>> 
```
<!-- more -->
## python中队列的实现 ##
队列是一种先进先出（FIFO）的数据结构，在一端（队尾）插入元素，在另一端（队首）删除元素。而如果用列表实现这种数据结构不是很高效，原因在于在列表尾插入和删除元素是很快的（时间复杂度O(1)），而在列表头插入元素是很慢的（时间复杂度O(n)），因为在列表头部插入和删除元素，列表中其余所有元素都要移动。
为了实现队列，用`collections.deque`双端队列，可以在两端快速的插入和删除元素（时间复杂度都是O(1)）。
```python
>>> from collections import deque
>>> queue = deque(['A', 'B', 'C'])
>>> queue.append('D')
>>> queue.append('E')
>>> queue
deque(['A', 'B', 'C', 'D', 'E'])
>>> queue.popleft()
'A'
>>> queue.popleft()
'B'
>>> queue
deque(['C', 'D', 'E'])
>>> 
```
双端队列`collections.deque`也是基于列表实现的，由此可以看出基础的数据结构，在Python中通常无需我们自己实现，基于python现有的强大的数据类型，很容易构造出我们想要的数据结构。