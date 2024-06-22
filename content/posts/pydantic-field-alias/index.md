---
title: "关于Pydantic model字段的别名"
date: 2024-06-22T19:12:00+08:00
draft: false
---

## 给model字段指定别名

Pydantic的model字段名是通过Python的类型注解设定的。这样一来当schema的字段名不是合法的Python变量名的时候就会带来问题。这种情况下可以用Field对象的alias属性来给字段指定名称。例如下面的例子给`family_name`这个字段设定了别名`family-name`。

```python
from pydantic import BaseModel, Field


class Person(BaseModel):
    family_name: str = Field(alias='family-name')


json_str = '{"family-name": "Smith"}'
person = Person.model_validate_json(json_str)
print(person.model_dump_json())
```

上面的代码输出为：
```
{"family_name":"Smith"}
```

可见，默认设置下Pydantic输出时会使用本名而非别名。如果希望使用别名，可以设置参数`by_alias=True`，也就是把代码最后一行改成：
```python
print(person.model_dump_json(by_alias=True))
```

## 使用alias generator自动生成别名

当一个model包含很多字段的时候，逐个手动填写别名就会很麻烦。这时候可以用alias generator自动生成别名。一个最简单的alias generator就是一个str->str的函数。通过设置model的alias generator参数就可以自动生成别名：
```python
from pydantic import BaseModel, ConfigDict


def my_alias_generator(name: str) -> str:
    return name.replace('_', '-')


class Person(BaseModel):
    model_config = ConfigDict(alias_generator=my_alias_generator)
    family_name: str
    given_name: str


json_str = '{"family-name": "Smith", "given-name": "Alice"}'
person = Person.model_validate_json(json_str)
print(person.model_dump_json(by_alias=True))
```

`pydantic.alias_generators`模块中已经提供了一些常见的alias generator，可以直接使用。
