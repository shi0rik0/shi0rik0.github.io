---
title: "Python循环语句中的else子句"
date: 2024-05-16T20:23:06+08:00
draft: false
---

Python 的循环语句（while 和 for 语句）有一个特殊的语法，就是它们支持 else 子句。下面用一个例子来说明其作用。

```python
for i in a:
    if find(i):
        do_something(i)
        break
else:
    not_found()
```

在这个代码片段中，我们尝试在容器`a`中寻找第一个符合某种条件的元素，然后对这个元素进行某些处理。如果在容器中没有找到符合条件的元素，就执行另外的逻辑。

简单来说，当一个循环语句是正常结束的时候（也就是说不是通过 break 中止的），else 子句才会执行。

如果不用 else 子句，那么就必须引入一个额外的 bool 变量才能实现相同的逻辑：

```python
found = False
for i in a:
    if find(i):
        found = True
        do_something(i)
        break
if not found:
    not_found()
```

最后指出一点，else 子句早在 Python 2 就已经被支持了，这不是一个最近才添加的语法糖，所以这个特性适用于几乎所有 Python 版本。
