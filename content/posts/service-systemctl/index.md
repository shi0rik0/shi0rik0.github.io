---
title: "关于SysVinit和systemd"
date: 2024-09-14T17:07:00+08:00
draft: false
---

Linux 管理服务的系统有两种：SysVinit 和 systemd。systemd 比 SysVinit 推出得更晚。SysVinit 本质上就是运行一些管理服务的 shell 脚本，轻量灵活但是简陋，而 systemd 则是一个封装得更加复杂的系统。现在大多数 Linux 发行版都已经使用 systemd 了，但是在 Docker 环境下，由于种种原因，大多数情况下还是会使用 SysVinit。因此，同时掌握这两种系统还是很有必要的。

## 用 systemd 管理服务

systemd 管理服务的命令是`systemctl`。

启动或关闭某个服务：

```bash
sudo systemctl start $SERVICE
sudo systemctl stop $SERVICE
```

查看某个服务的状态：

```bash
sudo systemctl status $SERVICE
```

管理服务的开机启动：

```bash
sudo systemctl enable $SERVICE
sudo systemctl disable $SERVICE
```

## 用 SysVinit 管理服务

使用`service`命令启动或关闭某个服务：

```bash
sudo service $SERVICE start
sudo service $SERVICE stop
```

下面介绍如何在 SysVinit 下管理服务的开机启动。由于 SysVinit 大多是在 Docker 环境下使用，而在 Docker 环境下其实没有所谓开机启动的概念，所以下面的内容可以跳过。

使用`update-rc.d`命令开机启动某个服务：

```bash
sudo update-rc.d $SERVICE defaults
```

关闭开机启动：

```bash
sudo update-rc.d -f $SERVICE remove
```
