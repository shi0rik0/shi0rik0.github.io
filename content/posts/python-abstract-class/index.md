---
title: "在Python中更优雅地创建抽象类及接口"
date: 2025-01-05T19:21:11+08:00
draft: false
---

以前我创建抽象类或者接口的时候，通常会这样写：

```python
class AbstractClass:
    def method(self, arg: int) -> int:
        raise NotImplementedError()
```

这么做会带来以下问题：

1. 如果子类没有实现抽象方法，那么在调用该方法的时候才会报错，而不是在创建对象的时候就报错。
2. 无法强制子类按照 type hints 的要求来实现方法。

下面就介绍一下如何解决这两个问题。

## 强制子类实现抽象方法

Python 标准库提供了一个类`abc.ABC`，如果一个类继承了`abc.ABC`，那么这个类就是一个抽象类。我们可以使用`@abc.abstractmethod`装饰器来标记一个方法是抽象方法。抽象类的子类必须实现所有抽象方法，否则在创建对象的时候就会报错。

```python
from abc import ABC, abstractmethod

class AbstractClass(ABC):
    @abstractmethod
    def method(self, arg: int) -> int:
        ...

class ConcreteClass(AbstractClass):
    def method(self, arg: int) -> int:
        return arg

c = ConcreteClass() # 没问题
a = AbstractClass() # 会得到 TypeError
```

## 确保子类正确 override 方法

从 Python 3.12 开始，`typing`模块新增了一个`@override`装饰器。这个装饰器可以提醒静态检查器这个方法必须覆盖父类的方法，并且方法签名必须一致。

```python
from typing import override

class AbstractClass:
    def method(self, arg: int) -> int:
        raise NotImplementedError()

class ConcreteClass(AbstractClass):
    @override
    def method(self, arg: int) -> None: # 会引发静态检查器的警告
        pass
```

如果是 Python 3.11 及以下的版本，可以用`typing_extensions`模块提供的 backport。
