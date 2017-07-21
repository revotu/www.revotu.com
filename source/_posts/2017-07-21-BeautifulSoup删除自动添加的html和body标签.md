---
title: BeautifulSoup删除自动添加的html和body标签
date: 2017-07-21 10:54:54
tags: [BeautifulSoup]
permalink: remove-html-and-body-with-beautifulsoup
---
最近用bs4解析html文档时，遇到个问题，当传入BeautifulSoup是一小段html片段时，会自动加上`<html>`和`<body>`标签，但实际上，我是不需要自动加的`<html>`和`<body>`标签，总结了几种删除他们的方法。
## 问题描述 ##
```python
from bs4 import BeautifulSoup

fragment = BeautifulSoup('<div><p>soup 4</p></div>','lxml')

print(fragment)
```
输出结果是`<html><body><div><p>soup 4</p></div></body></html>`。
<!-- more -->
## 解决方法一：unwrap方法 ##
```python
from bs4 import BeautifulSoup

fragment = BeautifulSoup('<div><p>soup 4</p></div>','lxml')

fragment.html.unwrap()
fragment.body.unwrap()

print(fragment)
```
## 解决方法二：next_element 属性 ##
```python
from bs4 import BeautifulSoup

def remove_html_and_body(soup):
    if soup.body:
        return soup.body.next_element
    elif soup.html:
        return soup.html.next_element
    else:
        return soup

fragment = BeautifulSoup('<div><p>soup 4</p></div>','lxml')

fragment = remove_html_and_body(fragment)

print(fragment)
```
## 解决方法三：简单提取 ##
如果确定要提取的结果很简单，就是一个`div`，那么直接提取这个`tag`即可。但要注意要仔细分析需求，因为此方法很局限不通用。
```python
from bs4 import BeautifulSoup

fragment = BeautifulSoup('<div><p>soup 4</p></div>','lxml')

fragment = fragment.div

print(fragment)
```