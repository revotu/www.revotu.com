---
title: npm install时的--save选项有什么用
date: 2016-09-07 15:49:43
tags: [NPM]
permalink: what-is-the-save-option-for-npm-install
---
## 在用NPM安装package时，有时会带`--save`选项，那它到底有什么用呢？ ##
默认情况下，NPM只需在`node_modules`下安装你要安装的package，但是这样，我们就需要手动添加`package.json`文件中的`dependencies`部分。
`--save`选项的作用就是告诉NPM安装完package后把`package.json`文件中的`dependencies`部分也同时更新了。
另外，`--save-dev`和`--save-optional`这两个选项，分别是代表更新`package.json`文件中的`devDependencies`和`optionalDependencies`部分。
<!-- more -->
## 在`dependencies`下添加package##
```
npm install my_dep --save
```
或是
```
npm install my_dep -S
```
## 在`devDependencies`下添加package ##
```
npm install my_test_framework --save-dev
```
或是
```
npm install my_test_framework -D
```
## 最终，package.json文件如下 ##
![package.json](/uploads/articles/4/npm-save.png)