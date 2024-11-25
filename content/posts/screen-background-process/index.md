---
title: "使用screen命令在后台运行进程"
date: 2024-04-26T17:01:50+08:00
draft: false
---

谈到在后台运行进程，大多数人第一个想到的可能是直接在命令后面加一个&，这么做有一些缺点：

1. 需要强行终止进程的时候，必须记住或者查询进程的名称、ID 等等。
2. 有时候使用&似乎会导致一个奇怪的 bug，使得终端输出紊乱甚至无法使用（参见[这个 StackOverflow 问题](https://askubuntu.com/questions/1459049/bash-script-launching-background-process-breaks-terminal-output-and-kills-backgr)）。

因此，我发现使用 screen 命令是一种更好的解决方案。

## 使用 screen 创建后台进程

示例命令：

```
screen -dmS alice sleep 10
```

这个命令中，sleep 10 是要执行的命令，-dm 表示在后台运行而不是进入 screen session，-S 表示我们要给这个 session 起一个名字，这里我起名为 alice。

## 使用 screen 终止后台进程

示例命令：

```
screen -S alice -X quit
```

这个命令中，-S alice 指定了我们之前创建的名为 alice 的 session，而-X quit 表示让这个 session 执行 quit 命令（也就是退出）。
