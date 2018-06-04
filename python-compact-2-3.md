---
title: Python Compact 2 vs 3
---

### 兼容范围

    python2.7+ 以及 python3.5+

### 避免 API 断层

```
# 编码问题
# -*- coding: utf-8 -*-

# 整除问题 py2 取整 py3 取浮点
from __future__ import division

# 打印问题
from __future__ import print_function

# 文件读取问题
from io import open

with open("xxx.txt", encoding='utf-8') as f:
    for line in f:
        print(repr(line))

# 增加判别语句
try:
    import pathlib
except Exception:
    import pathlib2
```

### 工具

CI 工具，自动测试不同版本
