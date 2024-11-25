---
title: "docker compose up的build选项"
date: 2024-10-02T10:13:00+08:00
draft: false
---

从本地的 Dockerfile 构建镜像启动 Docker Compose 的时候（比如像下面的配置），

```yml
services:
  service0:
    build:
      context: .
      dockerfile: Dockerfile
```

一定要记得在`docker compose up`的时候加上`--build`的选项：

```sh
docker compose up --build
```

因为 Docker Compose 的缓存机制非常粗糙，它不会检查 Dockerfile 是否有更改，也不会检查构建过程中是否有缓存失效了，所以 99%的情况下不添加`--build`会导致修改无法生效。

总结：使用`docker compose up`的时候**一定要**加上`--build`的选项。
