---
title: '用setuptools打包资源文件'
date: 2025-01-13T19:08:00+08:00
draft: false
---

默认情况下，setuptools 只会打包 Python 源代码文件，但有时候我们还需要打包一些资源文件，比如配置文件、模板文件、图片等等。这篇文章将会介绍如何用 setuptools 打包资源文件，并在库执行的时候访问这些文件。

我们假设项目的目录结构如下：

```
myproject/
├── mypkg/
│   ├── __init__.py
│   └── data/
│       ├── template.txt
└── setup.py
```

## 指定要打包的资源文件

要打包 `mypkg/data/template.txt` ，我们需要在 `setup.py` 中添加如下代码：

```python
from setuptools import setup

setup(
    # ...
    include_package_data=True,
    package_data={'mypkg': ['data/template.txt']},
    # ...
)
```

`package_data`参数也可以使用 glob 模式，比如`mypkg': ['data/*.txt']`。

## 访问资源文件

可以使用`importlib.resources`模块来访问资源文件。这个模块是在 Python 3.7 中引入的。

使用`open_text`或者`open_binary`方法来打开资源文件：

```python
import mypkg
import importlib.resources

anchor = mypkg  # 也可以是字符串'mypkg'
with importlib.resources.open_text(anchor, 'data/template.txt') as f:
    print(f.read())
```
