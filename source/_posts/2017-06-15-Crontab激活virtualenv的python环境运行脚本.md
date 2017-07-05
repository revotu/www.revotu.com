---
title: Crontab激活virtualenv的python环境运行脚本
date: 2017-06-15 15:07:02
tags: [Python,Virtualenv]
permalink: calling-python-script-from-crontab-with-activate
---
## 方法一：设置python脚本运行时的解释器路径 ##
在python脚本行首加上解释器路径，crontab中正常启动脚本即可。
```Shell
#!/home/user/virtualenv/bin/python
```
## 方法二：crontab中设置启动python脚本的解释器路径 ##
```Shell
0 3 * * * /home/user/virtualenv/bin/python /home/user/project/manage.py
```
<!-- more -->
如果脚本所在的project路径需要加到`PYTHONPATH`中，则先cd到这个路径下在执行上述命令即：
```Shell
0 3 * * * cd /home/user/project && /home/user/virtualenv/bin/python /home/user/project/manage.py
```