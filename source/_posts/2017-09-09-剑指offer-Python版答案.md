---
title: 剑指offer-Python版答案
date: 2017-09-09 12:08:59
tags: [Python]
permalink: coding-interviews-python-solutions
---
最近在看剑指offer，原书都是用C++实现的，考虑用Python实现一遍所有问题答案。由于[牛客网](https://www.nowcoder.com/ta/coding-interviews)有剑指offer的在线评测，所以本文所有代码都基于牛客网上的问题（可能会和原书题目描述不同）实现。
<!-- more -->
## 字符串 ##
### 	替换空格 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)
> 请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

分析：
> Python中直接使用字符串的`replace`函数即可。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        return s.replace(' ', '%20')
```
### 正则表达式匹配 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/45327ae22b7b413ea21df13ee7d6429c?tpId=13&tqId=11205&tPage=3&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
> 请实现一个函数用来匹配包括`.`和`*`的正则表达式。模式中的字符`.`表示任意一个字符，而`*`表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

分析：
> 直接使用python中的正则表达式进行完全匹配即可解决此问题。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    # s, pattern都是字符串
    def match(self, s, pattern):
        import re
        result = re.findall('^' + pattern + '$', s)
        if result:
            return True
        else:
            return False
```
### 表示数值的字符串 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/6f8c901d091949a5837e24bb82a731f2?tpId=13&tqId=11206&tPage=3&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
> 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

分析：
> 可以用float函数是否抛出`ValueError`异常来判断，或是直接写正则表达式。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    # s字符串
    def isNumeric(self, s):
        try:
            float(s)
        except ValueError:
            return False
        else:
            return True
```
## 数组 ##
### 数组中只出现一次的数字 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811?tpId=13&tqId=11193&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
> 一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

分析：
> 每次从列表中pop出一个值，如果这个值在列表中还存在，则直接从列表中remove掉这个值，如果不存在则加入结果列表中。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    # 返回[a,b] 其中ab是出现一次的两个数字
    def FindNumsAppearOnce(self, array):
        result = []
        while array:
            number = array.pop()
            if number not in array:
                result.append(number)
            else:
                array.remove(number)
        return result
```

### 二维数组中的查找 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&tqId=11154&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)
> 在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

分析1：
> 如果不考虑题目特点的话（每行每列都递增），直接迭代列表每一行，检测该值是否在每一行的列表即可。

代码1：
```python
# -*- coding:utf-8 -*-
class Solution:
    # array 二维列表
    def Find(self, target, array):
        for row in array:
            if target in row:
                return True
```
分析二：
> 由于题目特点，每行每列都递增，则可以每次比较最右上角的数字，如果相等则返回，输入的数大于右上角的数，则排除这一行，输入的数小于右上角的数，则排除这一列。

代码二：
```python
# -*- coding:utf-8 -*-
class Solution:
    # array 二维列表
    def Find(self, target, array):
        row = 0
        col = len(array[0]) - 1
        while row < len(array) and col >= 0:
            if array[row][col] == target:
                return True
            elif array[row][col] < target:
                row += 1
            else:
                col -= 1
        return False
```
## 递归和循环 ##
### 斐波那契数列 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3?tpId=13&tqId=11160&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
> 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。n<=39

分析：
> 在数学上，斐波那契数列是用递归的方法定义的：
> f(0) = 0 (n=0)
> f(1) = 1 (n=1)
> f(n) = f(n-1) + f(n-2) (n>=2)
> 如果程序中采用递归的方法话，那必定会超时的，因此要用循环的方法。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    def Fibonacci(self, n):
        prev, next = 0, 1
        for _ in range(n):
            prev, next = next, prev+next
        return prev
```