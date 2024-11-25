---
title: "Windows下用HTTP代理连接SSH"
date: 2024-04-23T10:44:18+08:00
draft: false
---

这里假设使用的是 Git 提供的 SSH。首先打开 Git Bash，输入命令

```
where connect
```

查找 connect.exe 的路径，这里假设是 C:\Program Files\Git\mingw64\bin\connect.exe。

然后打开 SSH 的配置文件，新增如下内容（将{hostname}:{port}替换成代理服务器地址）：

```
Host *
    ProxyCommand "C:\Program Files\Git\mingw64\bin\connect.exe" -H {hostname}:{port} %h %p
```

这样就可以让所有 SSH 连接都经过代理服务器了。

注意，这里 connect.exe 一定要使用绝对路径，哪怕将所在目录添加到了 PATH 中也不行。
