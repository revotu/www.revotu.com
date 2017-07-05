---
title: Nginx Rewrite简明指南
date: 2015-08-05 15:58:34
tags: [Nginx]
permalink: nginx-rewrite-concise-guide
---
## Nginx Rewrite 基本标记(flags) ##
> last - 相当于Apache里的[L]标记，表示完成rewrite，不再匹配后面的规则，将rewrite后的地址重新在server标签执行。
> break - 中止Rewrite，不再继续匹配，将rewrite后的地址重新在当前的location标签执行，它的生命周期在这个location中终结。
> redirect - 返回临时重定向的HTTP状态302
> permanent - 返回永久重定向的HTTP状态301

<!-- more -->
其中，last和break属于代理型，即在WEB服务器内部实现跳转。redirect和permanent属于跳转型，即客户端浏览器重新对地址进行请求。
原有的url支持正则，重写的url不支持正则。

## 正则表达式匹配 ##
> `~`          为区分大小写匹配
> `~*`         为不区分大小写匹配
> `!~`和`!~*`  分别为区分大小写不匹配及不区分大小写不匹配

## 文件及目录匹配 ##
> `-f`和`!-f` 用来判断是否是文件
> `-d`和`!-d` 用来判断是否是目录
> `-e`和`!-e` 用来判断是否是文件或目录
> `-x`和`!-x` 用来判断是否是可执行文件

## Nginx的一些可用的全局变量 ##
```
$args               #这个变量等于请求行中的参数。
$content_length     #请求头中的Content-length字段。
$content_type       #请求头中的Content-Type字段。
$document_root      #当前请求在root指令中指定的值。
$host               #请求主机头字段，否则为服务器名称。 如: www.revotu.com
$http_user_agent    #客户端agent信息
$http_cookie        #客户端cookie信息
$limit_rate         #这个变量可以限制连接速率。
$request_body_file  #客户端请求主体信息的临时文件名。
$request_method     #客户端请求的动作，通常为GET或POST。
$remote_addr        #客户端的IP地址。
$remote_port        #客户端的端口。
$remote_user        #已经经过Auth Basic Module验证的用户名。
$request_filename   #当前请求的文件路径，由root或alias指令与URI请求生成。
$query_string       #与$args相同。
$scheme             #HTTP方法（如http，https）。
$server_protocol    #请求使用的协议，通常是HTTP/1.0或HTTP/1.1。
$server_addr        #服务器地址，在完成一次系统调用后可以确定这个值。
$server_name        #服务器名称。
$server_port        #请求到达服务器的端口号。
$request_uri        #包含请求参数的原始URI，不包含主机名，如：”/foo/bar.php?arg=baz”。
$uri                #不带请求参数的当前URI，$uri不包含主机名，如”/foo/bar.html”。
$document_uri       #与$uri相同。
$request_time       #请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。
$time_iso8601       #ISO8601标准格式下的本地时间。
$time_local         #通用日志格式下的本地时间。
$http_referer       #引用地址
$upstream_response_time  #程序的响应时间(php-cgi的响应时间)
$body_bytes_sent    #Nginx返回给客户端的字节数，不含响应头。(服务器向客户端发送了多少的字节,日志统计时候加起来可以算出发送数据量的多少)
$bytes_sent         #Nginx返回给客户端的字节数, (这个每次都有值，因为有响应头的数据与$body_bytes_sent不同)
$request_length     #请求的长度（包括请求行，请求头和请求正文）
```