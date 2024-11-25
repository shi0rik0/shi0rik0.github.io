---
title: "关于Python的from __future__ import annotations"
date: 2024-05-20T19:05:17+08:00
draft: false
---

在默认情况下，Python 的 type hints 的值是真正的对象。请看下面的例子：

```python
def add(a: int, b: int) -> int:
    return a + b


print(add.__annotations__)

# output:
# {'a': <class 'int'>, 'b': <class 'int'>, 'return': <class 'int'>}
```

但这么做会引发一些问题，比如说考虑下面的代码：

```python
class Node:
    left: Node
    right: Node

    def __init__(self):
        pass
```

这段代码是无法运行的，因为执行`left: Node`的时候，`Node`对象还没有被创建。

`from __future__ import annotations`可以解决这个问题。一旦启用这个特性，type hints 存储的值就会变成一个字符串。只有调用`typing.get_type_hints()`之后才会解析字符串，返回真正的对象。请看下面的例子：

```python
from __future__ import annotations
import typing


def add(a: int, b: int) -> int:
    return a + b


print(add.__annotations__)
print(typing.get_type_hints(add))

# output:
# {'a': 'int', 'b': 'int', 'return': 'int'}
# {'a': <class 'int'>, 'b': <class 'int'>, 'return': <class 'int'>}
```

由于这个特性改变了 Python 默认的行为，可能会导致依赖默认行为的库无法正常运行，因此 Python 将这个特性设计为可选的。

顺带一提，启用这个特性还有一个好处，就是对内置容器类型（比如 list）进行 type hints 的时候可以直接写`list[int]`，而不用写`typing.List[int]`了。
