---
title: "给boto3增加type hints"
date: 2024-11-15T09:34:00+08:00
draft: false
---

最近使用`boto3`的时候，发现`boto3.client()`的返回值没有标注类型，导致开发的时候很不方便。我原先以为是`boto3`的开发者忘了添加，后来看了源代码才知道，`boto3.client()`返回的对象的 class 是在运行时动态生成的，所以很难进行 type hints。（这就是动态语言的坏处啊！）

好在有个开发者生成了`boto3`各种 client 的 stub，包名叫`boto3-stubs`。假如使用的是 S3 client，就输入：

```sh
pip install boto3-stubs[s3]
```

然后在 VSCode 里输入：

```python
client = boto3.client('s3')
```

应该就会看到`client`已经被自动推断出了类型。

假如我们需要给接受 S3 client 的函数参数标注类型，就要这么写：

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from mypy_boto3_s3.client import S3Client
else:
    S3Client = object

def f(x: S3Client):
    pass
```

`TYPE_CHECKING`是一个特殊常量，对于代码分析器，它的值为 True，正常运行时则为 False。加上`S3Client = object`的作用则是为了保证运行时`S3Client`依然是被定义的变量，不然执行时会报错。这个问题也可以通过加上`from __future__ import annotations`来解决。
