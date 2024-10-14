---
title: "Dockerfile中CMD和ENTRYPOINT的区别"
date: 2024-10-14T19:01:00+08:00
draft: false
---

在Dockerfile中有两个命令`CMD`和`ENTRYPOINT`，它们都可以用来指定Docker镜像启动时的默认命令，那么它们究竟有什么区别呢？

我在网上看到[一篇博客文章](https://aws.amazon.com/cn/blogs/opensource/demystifying-entrypoint-cmd-docker/)很好地解释了两者的区别。简而言之，最终运行`docker run`命令的时候，容器执行的命令会是ENTRYPOINT + CMD，但是有以下几点需要注意：

1. 如果ENTRYPOINT是字符串形式而非数组形式，那么CMD会被**忽略**。
2. Dockerfile中指定的ENTRYPOINT和CMD都可以在执行`docker run`命令的时候被修改。命令格式为：`docker run --entrypoint <ENTRYPOINT> <image> <CMD>`。

我引用的博客文章的作者推荐使用ENTRYPOINT，但我习惯只使用CMD，而不设置ENTRYPOINT，原因是这样比较简单，也不容易出错。
