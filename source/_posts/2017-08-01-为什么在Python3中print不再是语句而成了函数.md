---
title: 为什么在Python3中print不再是语句而成了函数
date: 2017-08-01 23:52:56
tags: [Python]
permalink: why-print-became-a-function-in-python-3
---
在Python 2中print是语句（statement），而在Python 3中print变成了函数（function）。也就是说Python 2中的`print 'hello'`，在Python 3中需要写成这样`print('hello')`。可能有人会说我在Python 2中也明明是把print当成函数用的：
```python
# python 2
>>> print('hello') #等价于 print ('hello') 也即 print 'hello'
hello

# python 3
>>> print('hello')
hello
```
<!-- more -->
从输出结果来看是一样的，但本质上，前者是把 ('hello')当作一个整体，而后者 print()是个函数，接收字符串作为参数。下面的例子更明显：
```python
# python 2
>>> print('hello', 'world')
('hello', 'world')

# python 3
>>> print('hello', 'world')
hello world
```
因为在Python 2中print不是函数，是语句。如果后面有括号，括号内没有逗号会被认为是数学表达式的括号，忽略。如果有逗号，则会被认为是元组。在Python 3中print是函数，所以后面必须有括号，可以接收可变长度参数。

如果希望在Python 2中把print当成函数用，可以在Python 2.6+中导入`__future__`模块中的`print_function`。
```python
# python 2
>>> print("hello", "world")
('hello', 'world')
>>>
>>> from __future__ import print_function
>>> print("hello", "world")
hello world
```
## print语句与print函数的区别 ##
### print语句 ###
在Python 2中，print语句最简单的使用形式就是`print A`，这相当于执行了`sys.stdout.write(str(A) + '\n')`。如果以逗号为分隔符，传递额外的参数（argument），这些参数会被传递至`str()`函数，最终打印时每个参数之间用一个空格拼接。

例如，`print A, B, C`相当于`sys.stdout.write(' '.join(map(str, [A, B, C])) + '\n')`。如果print语句的最后再加上一个逗号，那么就不会再添加换行符`\n`，也就是说：`print A,`相当于`sys.stdout.write(str(A))`。

从2.0版本开始，Python引入了`print >>`语法，目的是重定向输出。例如，`print >> output, A`相当于`output.write(str(A) + '\n')`。
### print函数 ###
print函数的等价定义如下：
```python
import sys

def print(*objects, sep=None, end=None, file=None, flush=False):  
    """A Python translation of the C code for builtins.print()."""
    
    if sep is None:
        sep = ' '
    if end is None:
        end = '\n'
    if file is None:
        file = sys.stdout
    file.write(sep.join(map(str, objects)) + end)
    if flush:
        file.flush()
```
从上面的代码中可以看出：Python 3中的print函数实现了Python 2中print语句的所有特性。
```python
print A                 等价于 print(A)
print A, B, C           等价于 print(A, B, C)
print A,                等价于 print(A, end='')
print >> output, A      等价于 print(A, file=output)
```
从上面的示例代码中可以看出，使用print函数有明显的好处：与使用print语句相比，我们现在能够指定其他的分隔符（separator）和结束符（end string）。
## print函数比print语句更灵活 ##
将print变成函数的核心原因在于print函数比print语句更灵活。print成为函数后，你可以把print当做表达式（expression）使用，相比之下，print语句就只能作为语句使用。

举个例子，假设你想在每一行后面打印一个省略号（ellipse），表示还有更多的东西未打印输出。使用print语句，你有两种选择：
```python
# 手动实现 ...
print A, '...'

# 可复用的实现（这种方式也适用于print函数） ...
def ellipsis_print(*args):
    for arg in args:
        print arg, '',
    print '...'
```
使用print函数，你可以选择更好的解决方式：
```python
# 手动实现 ...
print(A, end='...\n')

# 多个可复用的解决方案，利用print语句无法实现...
ellipsis_print = lambda *args, **kwargs: print(*args, **kwargs, end='...\n')
# 或者 ...
import functools
ellipsis_print = functools.partial(print, end='...\n')
```
换句话说，变成函数之后，print就可以组件化了，作为语句的print是无法支持的。还有，你还可以编写自己喜欢的print函数，将其赋值给`builtins.print`，就可以覆盖掉自带的print函数实现了。这一点在Python 2中是不可能实现的。

对于Python开发团队来说，他们不必再从语法层面来实现print的相关功能了。例如，如果你想让print语句也一样可以灵活地支持指定分隔符，你要怎样去实现呢？这会是一个相当难解决的设计难题。但是如果print变成了函数，只需要新增一个参数就解决了。在Python中，函数可以接受任意数量的参数，这比从底层实现语法带来的灵活性要大的多，比如print函数中的`flush`参数就是在Python 3.3中加入的，通过加一个参数`flush`很容易就扩展了print函数的功能，即是否刷新缓冲区，立即输出显示。

我们还要注意，语法实现应该仅限于那些非这样做不可的功能，或者是以语法形式实现后，大幅提高了可读性的功能。在print这个案例中，print A与print(A)之间的区别可以忽略不计，并没有影响可读性。而且，由于我们能够完全将print语句替换为函数，对于Python语言的功能性也没有损失。这就是为什么将print变成函数的原因。