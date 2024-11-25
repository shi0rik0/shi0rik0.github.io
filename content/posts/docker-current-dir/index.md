---
title: "关于Docker的当前目录"
date: 2024-09-20T14:23:00+08:00
draft: false
---

最近被 Docker 的当前目录位置整得有些混乱，特地在此整理一下。

## Docker Build

执行`docker build`命令的时候，需要传入一个 positional 参数，这个参数就是 context。举个例子，假如当前 shell 的目录是`/root`，我们执行以下命令：

```bash
docker build -f dockerfiles/a.Dockerfile dockerfiles
```

那么，构建过程中 Dockerfile 的 context 就是`/root/dockerfiles`。Context 的值会有以下三个影响：

1. Context 的值就是 Dockerfile 中`.`所代表的目录。
2. Docker 会将 context 目录下的所有文件（除了被.dockerignore 排除的）复制一遍。在构建镜像的时候，`COPY`命令只能访问到 context 下的文件。**这里有一个坑点**：假如我们将 context 设置成/root，那么在 Dockerfile 中访问`../tmp/a.txt`的时候不会立刻报错，而是会访问`/root/tmp/a.txt`。
3. Docker 在查找全局的.dockerignore 的时候，也是会在 context 目录下查找。但是这里又有一个坑点：Docker 还支持针对某个特定 Dockerfile 的.dockerignore，假设 Dockerfile 文件名为`foo.Dockerfile`，那么对应的.dockerignore 文件名就是`foo.Dockerfile.dockerignore`，但是这种.dockerignore 是在 Dockerfile 文件所在的目录下查找，而非 context 目录。举例来说，假如 Dockerfile 是`/root/dockerfiles/a.Dockerfile`，context 设置为`/root`，那么生效的.dockerignore 路径就是`/root/.dockerignore`以及`/root/dockerfiles/a.Dockerfile.dockerignore`。

## Docker Compose

Docker Compose 的当前目录是`docker-compose.yml`配置文件所在的目录，而**不是**执行`docker compose`命令时 shell 的当前目录，并且没有选项可以更改它。

但是这里又有一个坑点：在 Docker Compose 的配置文件中可以指定从本地的 Dockerfile 构建镜像，例如：

```yaml
services:
  service-0:
    build:
      context: ..
      dockerfile: docker/Dockerfile
```

这里面的`context`选项是相对于`docker-compose.yml`所在目录的，但是`dockerfile`选项却是相对于`context`的。也就是说，假如上面的配置文件位于`/foo/docker-compose.yml`，那么`context`就是`/`，而 Dockerfile 应该位于`/docker/Dockerfile`。
