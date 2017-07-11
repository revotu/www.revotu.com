---
title: Selenium Chrome 设置带用户名密码的proxy代理
date: 2017-07-10 16:37:57
tags: [Selenium,Python]
permalink: use-authenticated-proxy-in-selenium-chromedriver
---
## 设置公共的proxy代理方式 ##
由于公共的proxy不设有用户名密码，则可以直接使用chrome options的`--proxy-server=ip:port`参数启动chrome。
```python
import time
from selenium import webdriver

PROXY = '219.153.76.77:8080'
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--proxy-server={0}'.format(PROXY))
driver = webdriver.Chrome(chrome_options=chrome_options)
driver.get('http://httpbin.org/ip')
print(driver.page_source)

time.sleep(15)
driver.quit()
```
## 设置私有的proxy代理方式 ##
由于私有的proxy代理需要设置用户名密码，但chrome options并不支持`USERNAME:PASSWD@IP:PORT`这种方式，因此考虑用chrome插件实现自动代理用户名密码认证的方法。根据指定的代理`USERNAME:PASSWD@IP:PORT`自动创建一个Chrome代理插件，然后就可以在"Selenium + Chrome Driver"中通过安装该插件实现代理配置功能，具体代码如下。
```python
# coding: utf-8
# proxy.py
# "Selenium + Chrome"设置带用户名密码的proxy代理
import os
import re
import time
import zipfile
from selenium import webdriver

# Chrome代理模板插件地址: https://github.com/revotu/selenium-chrome-auth-proxy
CHROME_PROXY_HELPER_DIR = 'chrome-proxy-helper'
# 存储自定义Chrome代理扩展文件的目录
CUSTOM_CHROME_PROXY_EXTENSIONS_DIR = 'chrome-proxy-extensions'

def get_chrome_proxy_extension(proxy):
    """获取一个Chrome代理扩展,里面配置有指定的代理(带用户名密码认证)
    proxy - 指定的代理,格式: username:password@ip:port
    """
    m = re.compile('([^:]+):([^\@]+)\@([\d\.]+):(\d+)').search(proxy)
    if m:
        # 提取代理的各项参数
        username = m.groups()[0]
        password = m.groups()[1]
        ip = m.groups()[2]
        port = m.groups()[3]
        # 创建一个定制Chrome代理扩展(zip文件)
        if not os.path.exists(CUSTOM_CHROME_PROXY_EXTENSIONS_DIR):
            os.mkdir(CUSTOM_CHROME_PROXY_EXTENSIONS_DIR)
        extension_file_path = os.path.join(CUSTOM_CHROME_PROXY_EXTENSIONS_DIR, '{}.zip'.format(proxy.replace(':', '_')))
        if not os.path.exists(extension_file_path):
            # 扩展文件不存在，创建
            zf = zipfile.ZipFile(extension_file_path, mode='w')
            zf.write(os.path.join(CHROME_PROXY_HELPER_DIR, 'manifest.json'), 'manifest.json')
            # 替换模板中的代理参数
            background_content = open(os.path.join(CHROME_PROXY_HELPER_DIR, 'background.js')).read()
            background_content = background_content.replace('%proxy_host', ip)
            background_content = background_content.replace('%proxy_port', port)
            background_content = background_content.replace('%username', username)
            background_content = background_content.replace('%password', password)
            zf.writestr('background.js', background_content)
            zf.close()
        return extension_file_path
    else:
        raise Exception('Invalid proxy format. Should be username:password@ip:port')


if __name__ == '__main__':
    options = webdriver.ChromeOptions()
    # 添加一个自定义的代理插件（配置特定的代理，含用户名密码认证）
    options.add_extension(get_chrome_proxy_extension(proxy='username:password@ip:port'))
    driver = webdriver.Chrome(chrome_options=options)
    # 访问一个IP回显网站，查看代理配置是否生效了
    driver.get('http://httpbin.org/ip')
    print driver.page_source
    time.sleep(15)
    driver.quit()

```
其中用到的Chrome代理模板插件地址: [selenium and chrome set proxy authentication](https://github.com/revotu/selenium-chrome-auth-proxy)