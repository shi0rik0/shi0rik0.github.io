---
title: "Django外键的on_delete参数"
date: 2024-11-11T11:21:00+08:00
draft: false
---

在 Django 的 ORM 框架中，通过`models.ForeignKey`可以很方便地创建外键字段。请看下面的例子：

```python
from django.db import models

class Manufacturer(models.Model):
    name = models.TextField()

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer, on_delete=models.PROTECT)
```

在创建外键的时候，必须指定`on_delete`参数。在早期的 Django 版本中，这个参数有一个默认值`CASCADE`，而这个选项是非常危险的。假如数据库中有一些 Car 的 Manufacturer 是 BMW，那么如果设置了`CASCADE`，删除 BMW 就会将由 BMW 生产的所有 Car 的记录也一并删除，这样很可能导致数据丢失。如果设置为`PROTECT`，就会拒绝这次删除操作，并抛出一个异常。事实上，在生产环境中，往往很少直接删除记录，最多就是用一个专门的`deleted`字段来进行伪删除，因此将`on_delete`设置为`PROTECT`是我心目中的最佳实践。（`on_delete`还有一些安全性介于`CASCADE`和`PROTECT`之间的选项，例如`SET_NULL`和`RESTRICT`等等，这里就不赘述了。）
