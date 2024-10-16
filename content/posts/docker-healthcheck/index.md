---
title: "Docker Compose确保Postgres启动的方法"
date: 2024-10-16T13:12:00+08:00
draft: false
---

最近我编写了一个Django + Postgres的网站，其docker-compose.yml大概是下面这样的：
```yml
services:
  db:
    image: postgres

  web:
    image: django
    depends_on:
      - db
```

根据Docker文档，指定`depends_on`属性可以确保Django容器在Postgres容器启动之后才启动。但是这么设置之后，会发现启动的时候Django还是有可能连接不上Postgres导致异常。其原因在于：Postgres容器启动（也就是Postgres进程启动）**不代表Postgres进程已经准备好接受连接了**。这之间还需要一段时间用来初始化。因此上面的docker-compose.yml是不可靠的。

正确的写法应该是下面这样的：
```yml
services:
  db:
    image: postgres
    healthcheck:
      test: pg_isready || exit 1
      interval: 5s
      timeout: 10s
      retries: 120

  web:
    image: django
    depends_on:
      db:
        condition: service_healthy
```

首先看`db`的`healthcheck`属性。这个属性用来启用Docker的healthcheck机制。Docker会定期在容器内运行`test`所指定的命令，根据命令的返回值来判断容器的健康状态。`pg_isready`是Postgres官方提供的命令，用于检测Postgres是否已经完全启动。

这里有一个乍看之下比较奇怪的地方，就是在命令后面还加上了`|| exit 1`。这是因为，根据[Docker官方文档](https://docs.docker.com/reference/dockerfile/#healthcheck)，如果`test`所指定的命令的结束代码为0，则表示容器健康；若为1，则表示容器异常；结束代码2则作为保留值，不应该使用它（吐槽一下：那么除了0、1、2以外的结束代码呢？）。在Shell脚本中，`a || b`表示仅当`a`的结束代码为0时才执行`b`。因此当`pg_isready`返回0的时候，`pg_isready || exit 1`的结束代码是0；若`pg_isready`返回非0的值，则`pg_isready || exit 1`会一律返回1。这样就符合了Docker的约定。

然后在`web`的`depends_on`属性里加入`condition: service_healthy`，就可以让`web`容器在`db`容器处于healthy状态后才启动。
