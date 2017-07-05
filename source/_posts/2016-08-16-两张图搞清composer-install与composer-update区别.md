---
title: 两张图搞清composer install与composer update区别
date: 2016-08-16 18:50:04
tags: [Composer]
permalink: difference-between-composer-install-and-composer-update
---
<div align=center>
![composer](/uploads/articles/1/composer.png)
</div>
<!-- more -->
## composer install 与 composer update区别 ##
<div align=center>
![composer install](/uploads/articles/1/composer-install.jpg)
</div>
<div align=center>
![composer update](/uploads/articles/1/composer-update.jpg)
</div>
## 什么时候用composer install和composer update ##
+ composer update : 主要是在开发阶段使用，根据我们在composer.json文件中指定的内容升级项目的依赖包。
+ composer install : 主要是在部署阶段使用，以便在生产环境和开发环境使用的都是composer.lock文件中相同的依赖项，保证线上部署环境与本地开发环境的一致性。
**建议将composer.lock文件加入到版本控制(如：Git Repo)当中，以确保各开发环境的一致性。**