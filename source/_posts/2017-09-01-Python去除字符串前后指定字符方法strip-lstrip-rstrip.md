---
title: Python去除字符串前后指定字符方法strip() lstrip() rstrip()
date: 2017-09-01 15:43:09
tags: [Python]
permalink: removing-the-leading-and-trailing-characters-from-string
---
python中字符串去除指定字符，有三个非常简单的字符串方法：

> str.strip([chars])  : 去除字符串**前后两边**的指定字符。
> str.lstrip([chars]) : 去除字符串**前边**的指定字符。
> str.rstrip([chars]) : 去除字符串**后边**的指定字符。

<!-- more -->
如果没有传参数或者参数为None，则默认去除空白字符（空格，换行，制表符等等）：
```python
>>> s = ' \t hello world \n'
>>> s.strip()
'hello world'
>>> s.lstrip()
'hello world \n'
>>> s.rstrip()
' \t hello world'
>>> 
```
也可以指定参数，参数是一个字符串，代表要删除字符的集合：
```python
>>> s = '-----hello world====='
>>> s.strip('-=')
'hello world'
>>> s.lstrip('-')
'hello world====='
>>> s.rstrip('=')
'-----hello world'
>>> 
```
此外，需要注意的是，字符串是不可变对象，因此上面的函数生成的是新的字符串，原始字符串没有发生改变，如需改变原始字符串，赋值操作即可：
```
>>> s = ' \t hello world \n'
>>> s.strip()
'hello world'
>>> s
' \t hello world \n'
>>> s = s.strip()
>>> s
'hello world'
>>> 
```
另一个需要注意的是，去除字符的操作并不会对位于字符中间的任何文本起作用。例如：
```python
>>> s = '  hello \t world  \n'
>>> s.strip()
'hello \t world'
>>> 
```
如果要对里面的空格执行某种操作，应该使用其他技巧，比如使用`replace()`方法或正则表达式`re`替换。例如：
```python
>>> s = '  hello \t world  \n'
>>> s.replace(' ', '')
'hello\tworld\n'
>>> s.replace(' ', '').replace('\t', '').replace('\n', '')
'helloworld'
>>> 
>>> import re
>>> re.sub(r'\s+', '', s)
'helloworld'
>>> 
```