---
title: "Python 3.12的一个破坏兼容性的更改：移除distutils"
date: 2024-06-10T19:19:20+08:00
draft: false
---

最近使用Python 3.12 import keras的时候，遇到了如下的错误：
```
ModuleNotFoundError: No module named 'distutils'
```

根据[StackOverflow的热心网友的解释](https://stackoverflow.com/questions/77233855/why-did-i-get-an-error-modulenotfounderror-no-module-named-distutils)，这是因为Python 3.12不再预装distutils了，可以通过手动安装setuptools来安装distutils。但是我安装了setuptools后依然不能解决问题，因此最终被迫降级到3.11。

在我看来，这种对兼容性的破坏是没有必要的。目前我还是推荐使用Python 3.11。
