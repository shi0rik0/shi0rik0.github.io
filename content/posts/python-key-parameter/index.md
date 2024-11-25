---
title: "Python内置函数的key参数"
date: 2024-06-19T22:20:33+08:00
draft: false
---

Python 内置的 sort()、sorted()、max()和 min()都有一个可选的 key 参数。通过这个参数可以指定比较的基准。下面提供一些范例。

下面的代码会将 list 中的点按照 l2-norm 排序：

```python
a = [(3, 4), (1, 6), (2, 5)]
print(sorted(a, key=lambda x: x[0] ** 2 + x[1] ** 2))
```

下面的代码可以获取 list 中 l2-norm 最大的点：

```python
a = [(3, 4), (1, 6), (2, 5)]
print(max(a, key=lambda x: x[0] ** 2 + x[1] ** 2))
```
