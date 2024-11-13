---
title: "将本地机器的SSH密钥添加到远程机器的方法"
date: 2024-11-13T21:55:00+08:00
draft: false
---

用密码进行SSH登入麻烦又不安全。这篇文章将会教你如何将本地机器的SSH密钥添加到远程机器，用密钥快速登入SSH。

首先，如果是Windows系统，建议安装下Git for Windows，哪怕你不用Git。因为Git for Windows附带了SSH客户端工具，以及兼容Linux的Git Bash。

如果你的电脑上还没有SSH密钥，就输入下面的命令生成一个：
```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

然后运行下面的命令将本机密钥复制到远程机器上（Widnows下面要在Git Bash中运行，因为`ssh-copy-id`不是Windows可执行文件）：
```sh
ssh-copy-id username@remote_host
```

运行命令的时候，会让你输入密码登入远程机器。

这样就OK了，就是这么简单！
