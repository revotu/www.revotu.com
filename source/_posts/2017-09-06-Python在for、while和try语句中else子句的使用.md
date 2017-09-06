---
title: Python在for、while和try语句中else子句的使用
date: 2017-09-06 15:15:38
tags: [Python]
permalink: python-use-else-statement-in-for-while-try-statement
---
Python提供了一种很多编程语言都不支持的功能，那就是：else子句不仅能在if语句中使用，还能在for、while、和try语句中使用。
`for/else`、`while/else`和`try/else`的语义关系紧密，不过与`if/else`差别很大。
<!-- more -->
## `for/else`和`while/else`语句 ##
在for循环或是while循环正常运行完毕时（而不是通过break语句或是return语句或是异常退出循环），才会运行else块。
举个例子：
```python
>>> for i in range(3):
...     print(i)
... else:
...     print('Iterated over everything')
... 
0
1
2
Iterated over everything
>>> 
```
如上，for循环正常结束，所以运行了后面的else块。
```python
>>> for i in range(3):
...     if i == 2:
...         break
...     print(i)
... else:
...     print('Iterated over everything')
... 
0
1
>>> 
```
由此可以看出，for循环如果没有正常运行完毕（如上面是break结束循环的），是不会运行后面的else块。
当循环所迭代的对象是空的时候，else分支依然会被执行，毕竟循环仍然是正常完成的。
```python
>>> for i in []:
...     print(i)
... else:
...     print('Still iterated over everything (i.e. nothing)')
... 
Still iterated over everything (i.e. nothing)
```
同样不要忘记，以上所有也适用于`while/else`：
```python
>>> while i < 3:
...     print(i)
...     i += 1
... else:
...     print('Iterated over everything')
... 
0
1
2
Iterated over everything
>>> 
```
但是，我们为什么要在循环后面要用到else语句，有什么好处呢？
举个例子：
```python
images = ['a.jpg', 'b.png', 'c.gif', 'd.bmp']
found = False
for v in images:
    if v.endswith('.gif'):
        print('Found gif image')
        found = True
if not found:
    print('Not found gif image')
```
使用`for/else`语句：
```python
images = ['a.jpg', 'b.png', 'c.gif', 'd.bmp']
for v in images:
    if v.endswith('.gif'):
        print('Found gif image')
        break
else:
    print('Not found gif image')
```
由此可以看出，在这种循环时检测是否符合某个条件以便设置一个额外的标志变量的问题上，使用`for/else`语句可以更加pythonic的解决此问题。
## `try/else`语句 ##
仅当try块中没有异常抛出时才运行else块。一开始，你可能觉得没必要在`try/except`块中使用`else`子句。毕竟，在下述代码片段中，只有dangerous_call()不抛出异常，after_call()才会执行，对吧？
```python
try：
    dangerous_call()
    after_call()
except OSError:
    log('OSError...')   
```
然而，after_call()不应该放在try块中。为了清晰明确，try块中应该只包括抛出预期异常的语句。因此，向下面这样写更好：
```python
try:
    dangerous_call()
except OSError:
    log('OSError...')
else:
    after_call()
```
现在很明确，try块防守的是dangerous_call()可能出现的错误，而不是after_call()。而且很明显，只有try块不抛出异常，才会执行after_call()。但要注意一点，else子句抛出的异常不会由前面的except子句处理，也就是说此时after_call()如果抛出异常，将不会被捕获到。