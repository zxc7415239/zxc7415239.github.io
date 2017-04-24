---
title: Flush output of Python print
date: 2017-04-23 19:10:40
categories: Python
tags: Python
---

這邊紀錄如何將 print 的值 flush 到 output file 中。
<!--more-->
* [前言](#前言)
* [建置環境](#建置環境)
* [程式碼](#程式碼)
* [Reference](#Reference)

## 前言 <a href="#前言" id="前言">#</a>
這邊主要是提供一個將 print 值 flush 到一個檔案中的方法。

## 建置環境 <a href="#建置環境" id="建置環境">#</a>
OS: Windows 8.1
Python 2.7

## 程式碼 <a href="#程式碼" id="程式碼">#</a>
現在的結構如下
```
dir
├── func.py     # function
├── main.py     # 主程式
└── output.txt  # 將執行結果輸出到這裡
```
#### func.py
```
import sys
class flushfile(object):
    def __init__(self, f):
        self.f = f
    def __getattr__(self,name):
        return object.__getattribute__(self.f, name)
    def write(self, x):
        self.f.write(x)
        self.f.flush()
    def close(self):
        self.f.close()
import time
def sleep():
    for i in xrange(10):
        time.sleep(0.5)
        print 'test',i+1
```
#### main.py
```
import sys
from func import flushfile, sleep
#----------------------------#
orig_stdout = sys.stdout # open
sys.stdout = open(dir_path + "output.txt","w")
#----------------------------#
sys.stdout = flushfile(sys.stdout)
r = sleep()
#----------------------------#
sys.stdout.close() # close
sys.stdout = orig_stdout
#----------------------------#
```
#### output.txt
```
test 1
test 2
test 3
test 4
test 5
test 6
test 7
test 8
test 9
test 10
```
## Reference  <a href="#Reference" id="Reference">#</a>
[How to flush output of Python print?](http://stackoverflow.com/questions/230751/how-to-flush-output-of-python-print/231216#231216)
[[Python] \__getattr\__() 與 \__getattribute\__() 的差別](http://ephrain.pixnet.net/blog/post/60371176-%5Bpython%5D-__getattr__%28%29-%E8%88%87-__getattribute__%28%29-%E7%9A%84%E5%B7%AE%E5%88%A5)