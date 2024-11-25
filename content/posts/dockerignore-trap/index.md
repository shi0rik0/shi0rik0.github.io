---
title: "关于.dockerignore的陷阱"
date: 2024-09-24T15:25:00+08:00
draft: false
---

在 Docker 构建镜像的时候，默认会将 context 下的所有文件打包发送到 builder 进程。这是因为 Docker builder 采用了一种客户端/服务器的架构，这样就可以实现远程构建。但是如果 context 下面文件很大，就会影响构建速度。为此，Docker 提供了一个机制，可以通过在`.dockerignore`文件中指定要排除的文件。但是这个.dockerignore 的语法和.gitignore 有很大差异，因此很容易写错，其中一个陷阱就是，在.gitignore 中，`foo`等价于`**/foo`，而在.dockerignore 中，这等价于`/foo`。除此之外还有很多微妙的差异，比如[这篇文章](https://zzz.buzz/2018/05/23/differences-of-rules-between-gitignore-and-dockerignore/)中指出的：

> In .gitignore, if a path is listed as ignored (e.g. /dir), then any files or folders under that path is ignored and it is not possible to re-include them via negation (e.g. !/dir/file) as the parent directory of them is excluded;
>
> while in .dockerignore, even if a parent path is ignored, files or folders under it can still be re-included via negation.
