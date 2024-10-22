---
title: "Windows下Docker对路径斜杠处理的一个bug"
date: 2024-10-22T09:34:00+08:00
draft: false
---

最近在Windows下使用Docker的时候遇到了一个bug。起因是我需要将host上的一个文件挂载到Docker容器里，于是我就执行了以下命令：

```sh
docker run -v ./foo.txt:/tmp/foo.txt ubuntu
```

结果在容器里却无法打开`foo.txt`，后来发现Docker居然在容器里创建了一个空的名为`foo.txt`的目录！

后来我发现以下两种方法都可以解决问题：

1. 使用绝对路径。
2. 将斜杠改为Windows使用的反斜杠。

我推测这是因为Docker在Windows下对于路径斜杠处理存在bug。
