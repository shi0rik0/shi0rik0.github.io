---
title: "使用screen命令在后台运行进程"
date: 2024-04-26T17:01:50+08:00
draft: false
---

谈到在后台运行进程，大多数人第一个想到的可能是直接在命令后面加一个&，这么做有一些缺点：

1. 需要强行终止进程的时候，必须记住或者查询进程的名称、ID等等。
2. 有时候使用&似乎会导致一个奇怪的bug，使得终端输出紊乱甚至无法使用（参见[这个StackOverflow问题](https://askubuntu.com/questions/1459049/bash-script-launching-background-process-breaks-terminal-output-and-kills-backgr)）。

因此，我发现使用screen命令是一种更好的解决方案。

## 使用screen创建后台进程

示例命令：
```
screen -dmS alice sleep 10
```

这个命令中，sleep 10是要执行的命令，-dm表示在后台运行而不是进入screen session，-S表示我们要给这个session起一个名字，这里我起名为alice。


## 使用screen终止后台进程

示例命令：
```
screen -S alice -X quit
```

这个命令中，-S alice指定了我们之前创建的名为alice的session，而-X quit表示让这个session执行quit命令（也就是退出）。

