---
title: Python pandas 用法
date: 2017-05-02 16:08:01
categories: Python
tags: Python
---

這邊紀錄 pandas 常用指令。
<!--more-->
##### 讀檔案
```py
import pandas as pd
path = '/file'
df_excel = pd.read_excel(path + '/file.xlsx') # .xls 也可
df_csv = pd.read_csv(path + '/file.csv')
```
##### 二維 Keys 的 dictionary 轉換成 Dataframe
```py
mydict = {
    ('1st',1): 11,
    ('1st',2): 12,
    ('1st',3): 13,
    ('2nd',1): 21,
    ('2nd',2): 22,
    ('2nd',3): 23,
    ('3rd',1): 31,
    ('3rd',2): 32,
    ('3rd',3): 33,
}
data = map(list, zip(*mydict.keys())) + [mydict.values()]
df = pd.DataFrame(zip(*data)).set_index([0, 1])[2].unstack()
df.fillna(0, inplace=True) # missing data convert to zero
```
##### 結果
|Index  |1      |2      |3      |
|:-----:|:-----:|:-----:|:-----:|
|1st    |11     |12     |13     |
|2nd    |21     |22     |33     |
|3rd    |31     |32     |33     |
##### Dataframe columns 和 Index
```py
df.columns
df.index
```
##### Dataframe 轉換 type
```py
df.astype(int)
```
##### 自訂義 Index
有兩個參數能用
``inplace`` : 預設 ``False`` ，如果 ``True`` 就不會產生新的 Dataframe，會直接修改原先的 Dataframe。
``drop``    : 預設 ``True`` ，如果 ``False`` 就不會將 Dataframe 中該列移除掉。
```py
# 將上面的 df 拿來用
df = df.set_index(1, inplace=False, drop=True)
```
##### 結果
|Index  |2      |3      |
|:-----:|:-----:|:-----:|
|11     |12     |13     |
|21     |22     |33     |
|31     |32     |33     |
##### 輸出 Excel
```py
df.to_excel(path + '/file.xlsx', index=False)
# index 預設 True 會輸出 Index 那列
```