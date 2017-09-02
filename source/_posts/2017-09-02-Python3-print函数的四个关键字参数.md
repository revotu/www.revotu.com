---
title: Python3 print函数的四个关键字参数
date: 2017-09-02 11:35:56
tags: [Python]
permalink: python3-print-function-keyword-arguments
---
## print函数原型 ##
```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```
默认情况下，分隔符为空格`sep=' '`，行结尾符为换行符`end='\n'`，输出到标准输出`file=sys.stdout`，而且为带缓冲的`flush=False`。
<!-- more -->
## print函数自定义分隔符或行尾结束符 ##
print函数通过设置`sep`关键字参数自定义分隔符，`end`关键字参数自定义行尾结束符，例如：
```python
>>> print('www.revotu.com', 100, 66.6)
www.revotu.com 100 66.6
>>> print('www.revotu.com', 100, 66.6, sep=',')
www.revotu.com,100,66.6
>>> print('www.revotu.com', 100, 66.6, sep=',', end='!!\n')
www.revotu.com,100,66.6!!
>>> 
```
多数时候，不想要每次打印时行尾的换行符，设置`end=''`即可，例如：
```python
>>> for i in range(5):
...     print(i)
... 
0
1
2
3
4
>>> for i in range(5):
...     print(i, end='')
... 
01234>>> 
```
如果不知道print函数的`sep`关键字参数，很可能会写出如下丑陋的代码：
```python
print(a + ':' + b + ':' + c)   # Ugly
print(':'.join([a, b, c]))     # Still ugly
print(a, b, c, sep=':')        # Better
```
## print函数打印输出到文件 ##
设置`file`关键字参数为带有`write(string)`方法的对象即可，例如：
```python
with open('somefile.txt', 'wt') as f:
    print('hello world!', file=f)
```
确保文件是以文本模式`wt`打开的，如果文件是二进制模式打开的话，打印就会失败。
## print函数立即刷新输出 ##
设置`flush=True`即可不带缓冲的打印输出文本。
```python
print('some text', flush=True)
```