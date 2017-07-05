---
title: Python处理Windows环境下文件路径问题解决方案
date: 2017-07-04 17:21:52
tags: [Python]
permalink: python-handles-windows-file-path-solutions
---
> 经常会遇到Windows下的文件路径放到Python代码里出问题的事情，其原因就在于Windows下的路径分隔符是反斜线`\`，而和大多数语言一样，Python中的反斜线`\`是转义符。例如`\n`表示回车、`\t`表示制表符等等。所以，也就导致了Windows下的文件路径在Python中无法正常识别。
下面以Windows下文件路径`'C:\text\new'`为例，解决Windows下文件路径问题。

<!-- more -->
## 统一使用正斜线`/` ##
无论是在Windows下还是类UNIX系统下，路径分隔用正斜线`/`都适用。
```Python
'C:/text/new'
```
## 使用两个反斜线`\\` ##
由于反斜线是转义符，所以两个反斜线`\\`就表示一个反斜线`\`。
```Python
'C:\\text\\new'
```
## 使用Python中的raw string ##
Python中字符串前面加上字母`r`，表示是一个原始字符串(raw string)，即取消转义，列如`\n`此时不再表示回车，而是两个普通的字符`\`和`n`，通常用在正则表达式中。
```Python
r'C:\text\new'
```
但要注意：
```Python
filepath = r'C:\text\' + 'new'
```
上述代码是错误的，虽然字符串前面加上字母`r`使反斜线`\`失去了转义作用，但仍然有保护作用，其后的单引号`'`不会被视为closing delimiter。
## 路径拼接用`os.path.join(path, *paths)`方法 ##
Python中的os模块提供了很多操作文件和目录的方法，当在程序中拼接文件路径时推荐使用`os.path.join(path, *paths)`方法(第二个参数是*paths，可变长度参数列表)，避免了硬编码路径分隔符的问题。
```Python
filepath = os.path.join(dirpath, filename)
```
也可以使用`os.sep`，Python会根据不同系统选择合适的路径分隔符。
```Python
filepath = dirpath + os.sep + filename
```