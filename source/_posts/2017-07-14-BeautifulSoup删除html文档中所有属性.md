---
title: BeautifulSoup删除html文档中所有属性
date: 2017-07-14 15:13:19
tags: [BeautifulSoup]
permalink:  remove-all-html-attributes-with-beautifulsoup
---
## 删除html文档中所有属性 ##
```python
from bs4 import BeautifulSoup

# remove all attributes
def _remove_all_attrs(soup):
    for tag in soup.find_all(True): 
        tag.attrs = {}
    return soup
```
<!-- more -->
## 删除html文档中除了某些tag(`<a>` `<img>`)的所有属性 ##
```python
from bs4 import BeautifulSoup

# remove all attributes except some tags
def _remove_all_attrs_except(soup):
    whitelist = ['a','img']
    for tag in soup.find_all(True):
        if tag.name not in whitelist:
            tag.attrs = {}
    return soup
```
## 删除html文档中除了某些tag(`<a>` `<img>`)保留特定属性(`href` `src`)的所有属性 ##
```python
from bs4 import BeautifulSoup

# remove all attributes except some tags(only saving ['href','src'] attr)
def _remove_all_attrs_except_saving(soup):
    whitelist = ['a','img']
    for tag in soup.find_all(True):
        if tag.name not in whitelist:
            tag.attrs = {}
        else:
            attrs = dict(tag.attrs)
            for attr in attrs:
                if attr not in ['src','href']:
                    del tag.attrs[attr]
    return soup
```
