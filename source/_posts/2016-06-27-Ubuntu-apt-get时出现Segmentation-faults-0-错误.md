---
title: Ubuntu apt-get时出现Segmentation faults... 0%错误
date: 2016-06-27 11:24:41
tags: [ubuntu,Linux]
permalink: apt-get-command-ends-with-segmentation-fault
---
## apt-get时出现Segmentation faults... 0%错误 ##
执行如下命令，即可解决此问题。
```Shell
sudo rm -rf /var/cache/apt/*.bin
```
<!-- more -->