---
title: Redis下批量删除特定pattern的keys
date: 2015-04-23 22:51:35
tags: [Redis,NoSQL]
permalink: redis-delete-multiple-keys-by-pattern
---
## Redis下常用删除Key命令 ##
+ del：删除指定的单个key
+ flushdb：删除当前数据库的所有key
+ flushall：删除所有数据库的所有key

## Redis下批量删除特定pattern的keys ##
```Bash
redis-cli -h [host] -p [port] -n [db] KEYS "pattern" | xargs redis-cli -h [host] -p [port] -n [db] DEL
```
<!-- more -->
1. 如果访问的Redis在本地机器默认端口下，则可以省略 -h [host]以及 -p [port]选项。
2. 如果访问的Redis需要密码，则需要加上 -a [password]选项。
3. 访问Redis默认选中的db是0，所以如需访问特定db，命令中加入 -n [db]选项。
4. KEYS后指定的pattern需要用双引号括起来，否则无法删除。