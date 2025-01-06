---
title: "理清Python的包管理系统"
date: 2025-01-05T20:47:00+08:00
draft: false
---

现代的编程语言都会提供官方的包管理系统，例如 Rust 的 Cargo、Node.js 的 npm、Go 的 go mod 等等。Python 也不例外，但是 Python 的包管理系统非常的混乱，这里主要介绍一下 pip 和 setuptools 这两个工具。

## pip 和 setuptools 的关系

setuptools 是 Python 的**构建和打包工具**，它负责构建 Python 项目，并将其打包成一个 wheel 文件，而 pip 是 Python 的**包管理工具**，它可以直接安装 wheel 文件，也可以调用 setuptools 来打包项目并安装。pip 还负责管理包的依赖关系，它会在安装包的时候自动安装依赖。

setuptools 并不是 Python 官方支持的唯一打包工具，但它是最流行的，并且它的功能已经足够使用了，所以大部分 Python 项目都使用 setuptools 来打包项目。

## Python 项目的配置文件

由于历史问题，Python 项目的配置文件非常混乱，主要有 3 个：`setup.py`、`setup.cfg`和`pyproject.toml`。其中`setup.py`和`setup.cfg`是 setuptools 的配置文件。它们的区别在于，`setup.py`就是一个 Python 脚本，setuptools 会执行这个脚本，并根据脚本的变量值来配置项目。后来，setuptools 的维护者们认为这么做一方面不安全（因为脚本可以执行恶意代码），一方面也过于动态也会影响可复现性，所以他们引入了`setup.cfg`这个静态配置文件，一部分`setup.py`的配置可以通过`setup.cfg`来配置。而`pyproject.toml`是 PEP 518 提出的，它的目标是统一 Python 项目的配置文件，因为 Python 的打包工具不止 setuptools 一个，并且一个 Python 项目除了打包工具之外，还可能有其他需要配置的工具，例如 linters、formatters 等等。`pyproject.toml`是一个 TOML 格式的配置文件，Python 官方希望所有的工具都使用这个配置文件来配置项目，所以 setuptools 自然也跟进了，现在 setuptools 的配置也可以写在`pyproject.toml`中。

这里总结一下，`pyproject.toml`是一个统一的配置文件，可以配置任何工具，而`setup.py`和`setup.cfg`是 setuptools 的配置文件，用来配置 setuptools。

如果只是配置 setuptools，那么这 3 个都可以，但是`setup.py`明显是最灵活最强大的，所以我还是喜欢使用`setup.py`。又因为 Python 官方推荐使用`pyproject.toml`，所以我会在项目中同时使用`setup.py`和`pyproject.toml`，`setup.py`只负责配置 setuptools，而`pyproject.toml`主要负责指定项目的打包工具是 setuptools，以及配置其他工具。

下面的`pyproject.toml`会告诉 pip 使用 setuptools 来打包项目。一个 Python 项目的`pyproject.toml`至少要包含这些内容：

```toml
[build-system]
requires = ["setuptools >= 61.0"]
build-backend = "setuptools.build_meta"
```

这样一来，通过 pip 从源码安装包的时候，pip 就会调用 setuptools 来打包项目，而 setuptools 会综合`setup.py`、`setup.cfg`和`pyproject.toml`的配置来打包项目。
