---
title: "允许UWP应用访问localhost"
date: 2024-05-12T16:50:45+08:00
draft: false
---

由于 Windows 的安全限制，UWP 应用默认是不能访问 localhost 的。这会导致 UWP 应用无法翻墙。要允许 UWP 应用访问 localhost，就必须修改注册表。GitHub 上有人编写了一个 GUI 程序，可以帮助我们修改注册表来解除限制。原作者的仓库地址在[这里](https://github.com/tiagonmas/Windows-Loopback-Exemption-Manager)。可惜的是，原作者只提供了源代码，而没有提供编译好的可执行文件。不过好在 GitHub 上有人 fork 了仓库，提供了[编译好的可执行文件](https://github.com/piksel/Windows-Loopback-Exemption-Manager/releases/tag/v1.0.0.1)。
