---
title: Python抓路徑用法
date: 2017-04-25 12:38:56
categories: Python
tags: Python
---
這邊紀錄一些 Python抓路徑用法。
<!--more-->

#### 當前目錄
```py
import os
print os.path.dirname(os.path.abspath(sys.argv[0])).replace('\\','/')
print os.path.abspath(os.path.dirname(sys.argv[0])).replace('\\','/')
print os.getcwd()
```
#### 前一級目錄
```py
print os.path.abspath(os.path.join(os.path.dirname(sys.argv[0]), os.pardir))
```
#### 說明
```py
sys.argv[0]                    # 當前文件路徑
sys.path[0]                    # 當前文件路徑
__file__：                     # 當前文件路徑 可能會出問題
os.path.dirname(file):         # 某個文件所在的目錄路徑
os.path.join(a, b, c,....):    # 路徑構造 a/b/c
os.path.abspath(path):         # 將path從相對路徑轉成絕對路徑
os.pardir:                     # Linux下相當於"../"
```


