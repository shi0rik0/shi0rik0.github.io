---
title: "Python的代码格式化程序对比"
date: 2024-06-19T21:49:01+08:00
draft: false
---

知名的Python代码格式化程序主要有3个：[autopep8](https://github.com/hhatto/autopep8)、[black](https://github.com/psf/black)和[yapf](https://github.com/google/yapf)。其中autopep8是最古老的，它的开发已经不是很活跃了，因此不推荐使用。black和yapf的区别主要在于yapf提供了更多的格式化选项，让程序员可以根据自己的喜好调节代码风格。而black的作者有比较严重的洁癖，主张所有的Python代码都应该是统一的风格，因此几乎没有提供自定义选项。black甚至会强制要求字符串用双引号包裹。由于本人没有强迫症，因此还是更倾向于使用yapf。

在VSCode上就有一个现成的[名为yapf的插件](https://marketplace.visualstudio.com/items?itemName=eeyore.yapf)，只要安装这个插件就可以自动使用yapf格式化Python代码了。
