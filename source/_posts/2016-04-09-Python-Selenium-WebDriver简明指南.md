---
title: Python Selenium WebDriver简明指南
date: 2016-04-09 18:59:37
tags: [Python,WebDriver]
permalink: python-selenium-webdriver-concise-guide
---
## Google搜索 ##
```Python
from selenium import webdriver

CHROME_DRIVER_PATH = '../resources/chromedriver.exe'
driver = webdriver.Chrome(CHROME_DRIVER_PATH)
driver.get('https://www.google.com')

q = driver.find_element_by_name('q')
q.send_keys('webdriver')
q.submit()

print driver.title
driver.close()
```
<!-- more -->
## 刷新页面 ##
```Python
driver.refresh()
```
## 获得当前页面URL ##
```Python
driver.current_url
```
## 获取标签(如：p、div、span等)包含的文本 ##
```Python
element.text
```
## 等待元素动态加载完成 ##
**显式方法：**
```Python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Firefox()
driver.get("http://somedomain/url_that_delays_loading")
try:
    element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "myDynamicElement"))
    )
finally:
    driver.quit()

```
**隐式方法：**
```Python
from selenium import webdriver

driver = webdriver.Firefox()
driver.implicitly_wait(10) # seconds
driver.get("http://somedomain/url_that_delays_loading")
myDynamicElement = driver.find_element_by_id("myDynamicElement")
```
## 处理alert弹框 ##
```Python
try:
    WebDriverWait(driver, 3).until(EC.alert_is_present())
    alert = driver.switch_to_alert()
    alert.dismiss() #选择"否"，选择"是"的方法是alert.accept()
except:
    pass
```
***参考文档：[Selenium with Python](http://selenium-python.readthedocs.io/)***