---
title: Python开发环境复制迁移之requirements files
date: 2017-08-25 17:22:35
tags: [Python]
permalink: copy-python-environment-with-requirements-files
---
当python开发时需要环境复制迁移的时候，自动生成当前环境下的requirements files，此文件包含当前环境下安装的所有依赖包及其精确版本号，然后在新环境下安装时导入这个文件，即实现python开发环境的复制迁移。
## requirements files ##
```bash
$ env1/bin/pip freeze > requirements.txt
$ env2/bin/pip install -r requirements.txt
```
<!-- more -->
其中，`pip freeze > requirements.txt`命令导出当前环境下所有安装的依赖包及其版本号。
```bash
pip freeze > requirements.txt
```
然后，`pip install -r requirements.txt`命令在新环境下安装`requirements.txt`文件里面的所有依赖包。
```
pip install -r requirements.txt
```
更多关于pip的requirements files详细内容，请参考官方文档：[pip requirements files](https://pip.pypa.io/en/latest/user_guide/#requirements-files)