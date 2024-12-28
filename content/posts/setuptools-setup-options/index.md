---
title: "setuptools的setup()函数的常用参数"
date: 2024-12-28T20:09:03+08:00
draft: false
---

本文将介绍`setuptools`的`setup()`函数的常用参数，主要参考的是[官方文档](https://setuptools.pypa.io/en/latest/references/keywords.html)。

### name

包的名称，要将下划线替换为短横线，例如`my-package`。

### version

版本号，例如`1.2.3`。

### description

包的简要描述。

### long_description

包的详细描述，通常是从一个文件中读取的。

### long_description_content_type

`long_description`的类型，例如`text/markdown`。

### author

包的作者。

### author_email

作者的邮箱。

### url

包的主页，例如 GitHub 地址。

### packages

包含的包，通常不会手动填写，而是用`find_packages()`函数来查找。

### package_dir

包的目录，例如`{'': 'src'}`。

### classifiers

一个 list，包含包的分类信息，例如`Programming Language :: Python :: 3`。可选的分类信息可以在[PyPI](https://pypi.org/classifiers/)上查看。

### license

包的许可证，例如`MIT`。

### install_requires

运行时依赖的包的 list。

### entry_points

一个 dict，包含了包的入口点，例如`{'console_scripts': ['my-command = my_package.my_module:main']}`。假如安装了这个包，就可以通过命令行执行`my-command`命令，这个命令会调用`my_package.my_module`模块的`main()`函数。

### python_requires

指定 Python 的版本，例如`>=3.6`。

## 示例

```python
from setuptools import setup, find_packages

setup(
    name='my-package',
    version='1.2.3',
    description='A short description',
    long_description=open('README.md').read(),
    long_description_content_type='text/markdown',
    author='Author Name',
    author_email='xxx@example.com',
    url='https://xxx.com',
    packages=find_packages('src'),
    package_dir={'': 'src'},
    classifiers=[
        'Programming Language :: Python :: 3',
    ],
    license='MIT',
    install_requires=[
        'requests',
    ],
    entry_points={
        'console_scripts': [
            'my-command = my_package.my_module:main',
        ],
    },
    python_requires='>=3.6',
)
```
