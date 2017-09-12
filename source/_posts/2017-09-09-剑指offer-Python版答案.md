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
题目描述：[在线编程](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423)
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
题目描述：[在线编程](https://www.nowcoder.com/practice/45327ae22b7b413ea21df13ee7d6429c)
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
题目描述：[在线编程](https://www.nowcoder.com/practice/6f8c901d091949a5837e24bb82a731f2)
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
题目描述：[在线编程](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811)
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
题目描述：[在线编程](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e)
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
## 栈和队列 ##
### 用两个栈实现队列 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6)

> 用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

分析：
> 两个栈，一个作为队首，一个作为队尾，队列Push操作就是向队尾栈添加元素，队列Pop操作，需要先判断队首栈是否为空，如不空，则队首栈栈顶元素出栈，如为空，则将队尾栈所有元素依次pop出来，并push到队首栈，然后再队首栈顶元素出栈。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.front = []
        self.rear = []
 
    def push(self, node):
        self.rear.append(node)
 
    def pop(self):
        if not self.front:
            while self.rear:
                self.front.append(self.rear.pop())
        return self.front.pop()
```
### 包含min函数的栈 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49)

> 定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。

分析：
> 借助一个辅助栈，存储当前栈中最小元值组成的栈，入栈时小于等于辅助栈顶值时则同时也入辅助栈，出栈时，如等于辅助栈栈顶值，则辅助栈同步出栈这个值。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.stack = []
        self.min_stack = []
     
    def push(self, node):
        if not self.min_stack or node <= self.min_stack[-1]:
            self.min_stack.append(node)
        self.stack.append(node)
     
    def pop(self):
        if self.top() == self.min_stack[-1]:
            self.min_stack.pop()
        self.stack.pop()
     
    def top(self):
        return self.stack[-1]
     
    def min(self):
        return self.min_stack[-1]
```
### 栈的压入、弹出序列 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106)

> 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

分析：
> 借助一个辅助栈实际模拟入栈出栈，最后看辅助栈是否为空即可。

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    def IsPopOrder(self, pushV, popV):
        stack = []
        index = 0
 
        for v in pushV:
            stack.append(v)
            while stack and stack[-1] == popV[index]:
                stack.pop()
                index += 1
        return len(stack) == 0
```

## 递归和循环 ##
### 斐波那契数列 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3)
> 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。n<=39

分析：
> 在数学上，斐波那契数列是用递归的方法定义的：
> F（0）= 0 （n = 0）
> F（1）= 1 （n = 1）
> F（n）= F（n-1）+ F（n - 2）（n >= 2）
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
### 跳台阶 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4)

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

分析：
> F（0）= 0
> F（1）= 1
> F（2）= 2
> F（n）= F（n-1）+ F（n-2）（n > 2）

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloor(self, number):
        if number <= 2:
            return number
        prev, curr = 1, 2
        for _ in range(3, number+1):
            prev, curr = curr, prev+curr
        return curr
```
### 	变态跳台阶 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387)

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

分析：
> F（0）= 0
> F（1）= 1
> F（2）= 2
> F（n）= F（n-1）+ F（n-2）+ ... + F（2）+ F（1）+ 1 = 2 **（n-1）

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloorII(self, number):
        return 2**(number-1)
```
### 矩形覆盖 ###
题目描述：[在线编程](https://www.nowcoder.com/practice/72a5a919508a4251859fb2cfb987a0e6)

> 我们可以用`2*1`的小矩形横着或者竖着去覆盖更大的矩形。请问用n个`2*1`的小矩形无重叠地覆盖一个`2*n`的大矩形，总共有多少种方法？

分析：
> F（0）= 0
> F（1）= 1
> F（2）= 2
> F（n）= F（n-1）+ F（n-2）（n > 2）

代码：
```python
# -*- coding:utf-8 -*-
class Solution:
    def rectCover(self, number):
        if number <= 2:
            return number
        prev, curr = 1, 2
        for _ in range(3, number+1):
            prev, curr = curr, prev+curr
        return curr
```