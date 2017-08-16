---
title: Python中json转成html table
date: 2017-08-16 19:25:24
tags: [Python]
permalink: converting-json-to-html-in-python
---
最近，项目中有个需求，将json数据转成html table的形式。起初打算自己写个模块的，但是一搜索别人都已经写好了，完全满足需求，下面介绍一下方法。
## 安装json2html ##
```bash
pip install json2html
```
<!-- more -->
## 用法举例 ##
例1，基础用法：
```python
from json2html import *
input = {
        "name": "json2html",
        "description": "Converts JSON to HTML tabular representation"
}
json2html.convert(json = input)
```
输出结果：
```
<table border="1">
   <tr>
      <th>name</th>
      <td>json2html</td>
   </tr>
   <tr>
      <th>description</th>
      <td>converts JSON to HTML tabular representation</td>
   </tr>
</table>
```
例2，为table设置自定义属性：
```python
from json2html import *
input = {
        "name": "json2html",
        "description": "Converts JSON to HTML tabular representation"
}
json2html.convert(json = input, table_attributes="id=\"info-table\" class=\"table table-bordered table-hover\"")
```
输出结果：
```
<table id="info-table" class="table table-bordered table-hover">
   <tr>
      <th>name</th>
      <td>json2html</td>
   </tr>
   <tr>
      <th>description</th>
      <td>Converts JSON to HTML tabular representation</td>
   </tr>
</table>
```
其它更详细更高级用法，访问：[https://github.com/softvar/json2html](https://github.com/softvar/json2html)