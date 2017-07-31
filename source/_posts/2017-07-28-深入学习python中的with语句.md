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
## 自定义上下文管理器 ##
任何实现了`__enter__()`和`__exit__()`方法的对象都可以用于上下文管理器。
+ `context_manager.__enter__()`：进入上下文管理器的运行时上下文，在语句体执行前调用。with 语句将该方法的返回值赋值给 as 子句中的 target，如果指定了 as 子句的话。
+ `context_manager.__exit__(exc_type, exc_value, exc_traceback)`：退出与上下文管理器相关的运行时上下文，返回一个布尔值表示是否对发生的异常进行处理。参数表示引起退出操作的异常，如果退出时没有发生异常，则3个参数都为None。如果发生异常，返回True 表示不处理异常，否则会在退出该方法后重新抛出异常以由 with 语句之外的代码逻辑进行处理。如果该方法内部产生异常，则会取代由 with-body 中语句产生的异常。要处理异常时，不要显示重新抛出异常，即不能重新抛出通过参数传递进来的异常，只需要将返回值设置为 False 就可以了。之后，上下文管理代码会检测是否`__exit__()`失败来处理异常。

下面以数据库事务为例，讲解如何自定义上下文管理器。注意，上下文管理器必须同时提供`__enter__()` 和`__exit__()`方法的定义，缺少任何一个都会导致 `AttributeError`。with 语句会先检查是否提供了`__exit__()` 方法，然后检查是否定义了`__enter__()` 方法。
假设有个对象可以代表数据库连接，我们的目标是可以让使用者可以这样写代码：
```python
db_connection = DatabaseConnection()
with db_connection as cursor:
    cursor.execute('insert into ...')
    cursor.execute('delete from ...')
    # ... more operations ...
```
如果with语句体中的所有操作都执行成功，则事务被提交，否则有异常出现，则回滚代码，所有操作都不执行。这是我认为的`DatabaseConnection`的基本接口：
```python
class DatabaseConnection:
    # Database interface
    def cursor(self):
        "Returns a cursor object and starts a new transaction"
    def commit(self):
        "Commits current transaction"
    def rollback(self):
        "Rolls back current transaction"
```
此处的`__enter__()`方法是很简单的，仅仅是开始一个新的事务。返回的是`cursor`对象，以便可以用with语句中的`as cursor`绑定`cursor`这个变量名。
```python
class DatabaseConnection:
    ...
    def __enter__(self):
        # Code to start a new transaction
        cursor = self.cursor()
        return cursor
```
此处的`__exit__()`方法相对来说是复杂的，因为在这里完成了要做的大部分事情。`__exit__()`方法必须检查是否有异常出现，如果有没有异常，则事务被提交，如果有异常，则事务被回滚。
下面的代码中，`__exit__()`方法执行完的返回值是`None`。`None`在逻辑判断时也表示false，因此有异常将会自动向外层抛出。
```python
class DatabaseConnection:
    ...
    def __exit__(self, type, value, tb):
        if tb is None:
            # No exception, so commit
            self.commit()
        else:
            # Exception occurred, so rollback.
            self.rollback()
            # return False
```
## contextlib 模块 ##
contextlib 模块提供了3个对象：装饰器 contextmanager、函数 nested 和上下文管理器 closing。使用这些对象，可以对已有的生成器函数或者对象进行包装， 使其支持上下文管理器，避免了专门编写上下文管理器来支持 with 语句。
### 装饰器 contextmanager ###
contextmanager 用于对生成器函数进行装饰，生成器函数被装饰以后，返回的是一个上下文管理器，其`__enter__()` 和`__exit__()` 方法由 contextmanager 负责提供，生成器函数中 yield 之前的语句在`__enter__()` 方法中执行，yield 之后的语句在`__exit__()` 中执行，而 yield 产生的值赋给了 as 子句中的 value 变量。用这个装饰器，上面数据库事务的例子可以这么写：
```python
from contextlib import contextmanager

@contextmanager
def db_transaction(connection):
    cursor = connection.cursor()
    try:
        yield cursor
    except:
        connection.rollback()
        raise
    else:
        connection.commit()

db = DatabaseConnection()
with db_transaction(db) as cursor:
    ...
```
### 函数 nested ###
nested 可以将多个上下文管理器组织在一起，避免使用嵌套 with 语句。语法如下：
```python
with nested(A(), B(), C()) as (X, Y, Z):
     # with-body code here
```
类似于下面的执行过程：
```python
with A() as X:
    with B() as Y:
        with C() as Z:
             # with-body code here
```
Python 2.7和以后的版本不赞成使用nested(),因为可以直接嵌套：
```python
with A() as X, B() as Y, C() as Z:
    # with-body code here
```
### 上下文管理器 closing ###
closing 的实现如下：
```python
class closing(object):
    """Context to automatically close something at the end of a block.

    Code like this:

        with closing(<module>.open(<arguments>)) as f:
            <block>

    is equivalent to this:

        f = <module>.open(<arguments>)
        try:
            <block>
        finally:
            f.close()

    """
    def __init__(self, thing):
        self.thing = thing
    def __enter__(self):
        return self.thing
    def __exit__(self, *exc_info):
        self.thing.close()
```
上下文管理器会将包装的对象赋值给 as 子句的 target 变量，同时保证打开的对象在 with-body 执行完后会关闭掉（调用对象的close方法）。
```python
import urllib, sys
from contextlib import closing

with closing(urllib.urlopen('http://www.yahoo.com')) as f:
    for line in f:
        sys.stdout.write(line)
```
closing 适用于提供了 close() 实现的对象，比如网络连接、数据库连接等，也可以在自定义类时通过接口 close() 来执行所需要的资源“清理”工作。