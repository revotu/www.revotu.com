---
title: Python文件路径查找之glob模块
date: 2017-07-14 18:16:31
tags: [Python]
permalink: python-find-filepath-with-glob-module
---
python中的glob模块是用来查找匹配文件的，相比`os.listdir()`其可以支持模式匹配查找(Unix shell中的匹配规则)，还支持返回文件绝对路径。Unix shell中的匹配规则如下:
```shell
*       matches everything
?       matches any single character
[seq]   matches any character in seq
[!seq]  matches any char not in seq
```
<!-- more -->
## glob.glob(pathname) ##
返回所有匹配的文件路径列表。其只有一个参数pathname，定义了文件路径匹配规则，可以是绝对路径，也可以是相对路径。
```python
import glob

#返回的结果是绝对路径列表：指定目录下所有txt文件
print glob.glob('E:/task/*/*.txt')

#返回的结果是相对路径列表：上级目录下所有py文件
print glob.glob('../*.py')
```
## glob.iglob(pathname) ##
返回的是匹配的文件路径生成器，即可以使用它逐个获取匹配的文件路径名。与`glob.glob(pathname)`的区别是 : glob.glob(pathname)一次性获取所有的匹配路径，glob.iglob(pathname)一次只获取一个匹配路径。
```python
import glob

result = glob.iglob('../*.py')

print result #<generator object iglob at 0x02587648>

for file in result:
    print file
```
## 两点注意 ##
注意一：glob匹配规则中把以点`.`开头的文件作为特殊情况。假设当前路径下有两个文件`card.gif`和`.card.gif`
```python
import glob
>>> glob.glob('*.gif')
['card.gif']
>>> glob.glob('.c*')
['.card.gif']
```
注意二：glob匹配规则中默认不递归匹配文件夹，在python 3.5 +可以设置`recursive `，并在匹配规则中用`**`来实现递归匹配
```python
import glob

for filename in glob.iglob('src/**/*.c', recursive=True):
    print(filename)
```