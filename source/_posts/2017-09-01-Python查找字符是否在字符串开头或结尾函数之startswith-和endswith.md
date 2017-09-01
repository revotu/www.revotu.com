---
title: Python查找字符是否在字符串开头或结尾函数之startswith()和endswith()
date: 2017-09-01 16:39:22
tags: [Python]
permalink: find-whether-the-string-starts-with-prefix-ends-with-suffix
---
当需要检查字符串的开头或结尾时，只需使用`str.stratswith()`和`str.endswith()`方法就可以了，函数原型如下：
> str.startswith(prefix[, start[, end]]) : 检测字符串是否以某字符开头。
> str.endswith(suffix[, start[, end]]) : 检测字符串是否以某字符结尾。

<!-- more -->
## 参数为字符串 ##
示例如下：
```python
>>> filename = 'spam.txt'
>>> filename.endswith('.txt')
True
>>> url = 'http://www.revotu.com'
>>> url.startswith('http:')
True
```
## 参数为元组 ##
如需同时针对多个选项做检查，只需给`stratswith()`和`endswith()`提供包含可能选项的元组（tuple）即可：
```python
>>> import os
>>> filenames = os.listdir('.')
>>> filenames
['Makefile', 'foo.c', 'bar.py', 'spam.c', 'spam.h']
>>> [name for name in filenames if name.endswith(('.c', '.h'))]
['foo.c', 'spam.c', 'spam.h']
>>> any(name.endswith('.py') for name in filenames)
True
```
再举个例子，从文本文件中或是URL中读取文本内容：
```python
from urllib.request import urlopen

def read_data(name):
    if name.startswith(('http:', 'https:', 'ftp:')):
        return urlopen(name).read()
    else:
        with open(name) as f:
            return f.read()
```
需要注意的是，同时检查多个选项时，参数必须为元组，如果刚好我们把选项指定在了列表或集合中，确保首先用`tuple()`将其转换成元组。示例如下：
```python
>>> choices = ['http:', 'ftp:']
>>> url = 'http://www.revotu.com'
>>> url.startswith(choices)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: startswith first arg must be str or a tuple of str, not list
>>> url.startswith(tuple(choices))
True
```
此外，`stratswith()`和`endswith()`还有两个可选参数，`start`检测字符串的起始位置，`end`检测字符串的结束位置。
## 扩展 ##
`stratswith()`和`endswith()`提供了一种非常简单的方式来对字符串的前缀和后缀做基本的检查。类似的操作也可以用切片完成，但是那种方案不够优雅。例如：
```python
>>> filename = 'spam.txt'
>>> filename[-4:] == '.txt'
True
>>> url = 'http://www.revotu.com'
>>> url[:5] == 'http:' or url[:6] == 'https:' or url[:4] == 'ftp:' 
True
```
可能，我们也比较倾向于使用正则表达式作为替代方案。例如：
```python
>>> import re
>>> url = 'http://www.revotu.com'
>>> re.match(r'http:|https:|ftp:', url)
<_sre.SRE_Match object; span=(0, 5), match='http:'>
>>> 
```
这也行得通，但是通常对于简单的匹配来说有些过于重量级了。使用`stratswith()`和`endswith()`会更简单，运行得更快。
最后但同样重要的是，当`stratswith()`和`endswith()`和其他操作（比如常见的数据整理操作）结合起来时效果会更好。例如，下面的语句检查目录中有无出现特定的文件：
```
if any(name.endswith(('.c', '.h')) for name in os.listdir(dirname)):
    ... 
```