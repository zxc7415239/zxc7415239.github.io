---
title: Python 讀寫 JSON
date: 2017-04-30 16:58:22
categories: Python
tags: Python
---

讀寫 JSON。
<!--more-->

```py
import json
data = {'key': value}
# Writing JSON data
with open('data.json', 'w') as f:
    json.dump(data, f)

# Reading data back
with open('data.json', 'r') as f:
    data = json.load(f)
```

## Reference  <a href="#Reference" id="Reference">#</a>
[读写JSON数据](http://python3-cookbook.readthedocs.io/zh_CN/latest/c06/p02_read-write_json_data.html)