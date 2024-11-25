---
title: "关于Linux的service命令"
date: 2024-09-14T17:07:00+08:00
draft: false
---

`service`是 Linux 中一个用来管理服务的常用命令。它是一个高层命令，实际上调用的是底层的`systemctl`等命令。尽管`service`命令只支持对于服务的常用管理操作，但是对于绝大多数场合已经足够了。使用`service`命令还可以使我们不用关注底层命令的差异。

使用`service`命令列出当前所有服务的状态：

```bash
sudo service --status-all
```

使用`service`命令启动或关闭某个服务（例如 ssh）：

```bash
sudo service ssh start
sudo service ssh stop
```

下面的命令可以查看某个服务（例如 ssh）支持的所有操作：

```bash
sudo service ssh
```
