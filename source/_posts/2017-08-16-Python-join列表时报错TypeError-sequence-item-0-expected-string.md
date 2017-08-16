---
title: Python join列表时报错TypeError sequence item 0 expected string
date: 2017-08-16 18:31:14
tags: [Python]
permalink: python-join-list-type-error
---
## join语法 ##
```python
str.join(iterable)
```
`join`是python中**字符串**的方法，用于将序列中的元素以指定的字符连接生成一个新的字符串。
不仅是要求前面指定的分隔符是字符串类型，**后面要拼接的序列中每个元素也必须是字符串类型**。
如果在序列中出现非str类型，则报错：`TypeError`。
<!-- more -->
## 举例 ##
```python
data = [1, 2, 3]
print(', '.join(data))
```
报错：TypeError: sequence item 0: expected string, int found
原因：就在于要拼接的序列中，元素不是字符串类型。
解决：将序列中每个元素都转成str类型。
```python
data = [1, 2, 3]
print(', '.join(map(str, data)))
```
输出结果：
```python
1, 2, 3
```