---
title: "PowerShell脚本对于双横线的特殊处理"
date: 2025-01-15T20:11:00+08:00
draft: false
---

最近在运行`npm create vue@latest -- --help`的时候，发现一个有趣的现象：如果用 PowerShell 运行这个命令，会输出`npm create`的帮助信息，而用 cmd 运行就没有问题。后面经过调查，发现是 PowerShell 脚本对于双横线的特殊处理导致的。

npm 针对 cmd 和 PowerShell 提供了两个不同的脚本，分别是`npm.cmd`和`npm.ps1`。在 PowerShell 中运行`npm`的时候，实际上会执行`npm.ps1`。在 PowerShell 脚本中，`--`有特殊含义，因此执行`npm create vue@latest -- --help`就近似于执行`npm create vue@latest --help`，因此会输出`npm create`的帮助信息。

解决方法很简单，只需要将`--`用引号括起来即可，即`npm create vue@latest '--' --help`。这样 PowerShell 就不会对`--`进行特殊处理了。

具体可以参考这个[GitHub PR](https://github.com/vuejs/create-vue/pull/658)。

这里还要补充一点，这个问题只会出现在执行 PowerShell**脚本**的时候，用 PowerShell 执行普通二进制文件是不会有这个问题的。
