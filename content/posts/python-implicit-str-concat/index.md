---
title: 'Python的坑：隐式字符串连接'
date: 2025-01-13T20:19:20+08:00
draft: false
---

最近写 Python 代码的时候遇到了一个坑：Python 和 C 语言一样，支持隐式字符串连接。所谓隐式字符串连接，就是当两个字符串字面量相邻的时候，会自动连接成一个字符串。比如下面这段代码：

```python
s = 'hello' 'world'
assert s == 'helloworld'
```

利用这个语法可以将长字符串拆分成多行，但是也可能带来问题，比如下面这段代码：

```python
names = [
    'Alice'  # 这里漏了一个逗号
    'Bob',
    'Charlie',
]
```

这段代码的本意是创建一个包含 3 个人名的 list，但是由于缺少逗号，实际上创建的是一个只包含 2 个人名的 list。

好在 Pylint 可以检测这个问题，并给出警告 [W1404](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/implicit-str-concat.html)，但是 Pylint 默认不会警告跨行的字符串连接，所以还需要手动在 Pylint 的配置中加入以下内容：

```ini
[STRING_CONSTANT]
check-str-concat-over-line-jumps = true
```
