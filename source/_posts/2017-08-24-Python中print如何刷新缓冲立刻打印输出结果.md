---
title: Python中print如何刷新缓冲立刻打印输出结果
date: 2017-08-24 11:36:48
tags: [Python]
permalink: how-to-flush-output-of-python-print
---
python中用print打印输出时，在非交互式环境下，是有缓冲的（buffer），也就是不会立刻打印结果输出显示。如需刷新缓冲，强制print打印结果立刻显示，有以下几种方法。
## 通用方法 ##
```python
import sys
# some print codes
sys.stdout.flush()
```
在print语句后调用`sys.stdout.flush()`，强制立刻刷新缓冲。
<!-- more -->
## python 3.3+ ##
从python 3.3起，print函数增加了`flush`关键字参数，函数原型`print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)`，只需设置`flush`为True，即立刻刷新缓冲输出打印。
## 参数`-u` ##
可以再启动python脚本时，用`-u`选项，不启用缓冲。
```python
python -u script.py
```
或是，在脚本文件顶部设置
```
#!/usr/bin/env python -u
```