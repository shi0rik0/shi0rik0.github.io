---
title: "Windows下用HTTP代理连接SSH"
date: 2024-04-23T10:44:18+08:00
draft: false
---

这里假设使用的是Git提供的SSH。首先打开Git Bash，输入命令

```
where connect
```

查找connect.exe的路径，这里假设是C:\Program Files\Git\mingw64\bin\connect.exe。

然后打开SSH的配置文件，新增如下内容（将{hostname}:{port}替换成代理服务器地址）：
```
Host *
    ProxyCommand "C:\Program Files\Git\mingw64\bin\connect.exe" -H {hostname}:{port} %h %p
```

这样就可以让所有SSH连接都经过代理服务器了。

注意，这里connect.exe一定要使用绝对路径，哪怕将所在目录添加到了PATH中也不行。
