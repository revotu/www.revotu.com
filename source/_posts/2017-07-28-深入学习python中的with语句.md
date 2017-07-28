---
title: 深入学习python中的with语句
date: 2017-07-28 11:22:44
tags: [Python]
permalink: understanding-python-with-statement
---
## 引言 ##
大家在编程的时候，经常会遇到这样的场景：先执行一些准备操作，然后执行自己的业务逻辑，等业务逻辑完成以后，再执行一些清理操作。比如，打开文件，处理文件内容，最后关闭文件。又如，当多线程程序需要访问临界资源的时候，线程首先需要获取互斥锁，当执行完成并准备退出临界区的时候，需要释放互斥锁。对于这些情况，Python中提供了上下文管理器（Context Manager）的概念，可以通过上下文管理器来控制代码块执行前的准备动作以及执行后的清理动作，with语句就是基于上下文管理器工作的，更简洁的实现了`try...finally`语句块的功能。
<!-- more -->
## 概念 ##
上下文管理器（Context Manager）：实现了`__enter__()` 和` __exit__()`这两种方法的对象。上下文管理器定义执行 with 语句时要建立的运行时上下文，`__enter__()` 方法在语句体执行之前进入运行时上下文，`__exit__()` 方法在语句体执行完后从运行时上下文退出。

上下文表达式（Context Expression）：with 语句中跟在关键字 with 之后的表达式，该表达式要返回一个上下文管理器对象。

语句体（with-body）：with 语句包裹起来的代码块，在执行语句体之前会调用上下文管理器的` __enter__()` 方法，执行完语句体之后会执行 `__exit__()` 方法。
## 语法 ##
```python
with context_expression [as target]:
    with-body
```
这里 context_expression 要返回一个上下文管理器对象，该对象并不赋值给 as 子句中的 target，如果指定了 as 子句的话，会将上下文管理器的 `__enter__()` 方法的返回值赋值给 target。

Python 对一些内建对象进行改进，加入了对上下文管理器的支持，可以用于 with 语句中，比如可以自动关闭文件、线程锁的自动获取和释放等。假设要对一个文件进行操作，使用 with 语句可以有如下代码：
```python
with open(r'somefileName') as somefile:
    for line in somefile:
        print line
        # ...more code
```
这里使用了 with 语句，不管在处理文件过程中是否发生异常，都能保证 with 语句执行完毕后已经关闭了打开的文件句柄，而且此时的somefile就是`open()`创建的对象，原因是`file.__enter__()`返回的是`self`。如果使用传统的 try/finally 范式，则要使用类似如下代码：
```python
somefile = open(r'somefileName')
try:
    for line in somefile:
        print line
        # ...more code
finally:
    somefile.close()
```
比较起来，使用 with 语句可以减少编码量。已经加入对上下文管理器支持的还有模块 threading、decimal 等。

[PEP 0343](https://www.python.org/dev/peps/pep-0343/) 对 with 语句的实现进行了描述。with 语句的执行过程类似如下代码块：
```python
context_manager = context_expression
exit = type(context_manager).__exit__   #此时还未调用
value = type(context_manager).__enter__(context_manager)
exc = True   # True 表示正常执行，即便有异常也忽略；False 表示重新抛出异常，需要对异常进行处理
try:
    try:
        target = value  # 如果使用了 as 子句
        with-body     # 执行 with-body
    except:
        # 执行过程中有异常发生
        exc = False
        # 如果 __exit__ 返回 True，则异常被忽略；如果返回 False，则重新抛出异常
        # 由外层代码对异常进行处理
        if not exit(context_manager, *sys.exc_info()):
            raise
finally:
    # 正常退出，或者通过 statement-body 中的 break/continue/return 语句退出
    # 或者忽略异常退出
    if exc:
        exit(context_manager, None, None, None) 
    # 缺省返回 None，None 在布尔上下文中看做是 False
```
1. 执行 context_expression，生成上下文管理器 context_manager
2. 调用上下文管理器的 `__enter__() `方法；如果使用了 as 子句，则将` __enter__()` 方法的返回值赋值给 as 子句中的 target
3. 执行语句体 with-body
4. 不管是否执行过程中是否发生了异常，执行上下文管理器的 `__exit__()` 方法，`__exit__() `方法负责执行“清理”工作，如释放资源等。如果执行过程中没有出现异常，或者语句体中执行了语句 break/continue/return，则以 None 作为参数调用 `__exit__(None, None, None)` ；如果执行过程中出现异常，则使用 sys.exc_info 得到的异常信息为参数调用 `__exit__(exc_type, exc_value, exc_traceback)`

## 上下文管理器可以同时管理多个资源 ##
```python
with A() as a, B() as b, C() as c:
    doSomething(a,b,c)
```
假设你需要读取一个文件的内容，经过处理以后，写入到另外一个文件中。你能写出Pythonic的代码，所以你使用了上下文管理器，满意地写出了下面这样的代码：
```python
with open('input.txt', 'r') as source:
    with open('output.txt', 'w') as target:
        target.write(source.read())
```
你已经做得很好了，但是，上面这段代码还可以这么写：
```python
with open('input.txt', 'r') as source, open('output.txt', 'w') as target:
    target.write(source.read())
```