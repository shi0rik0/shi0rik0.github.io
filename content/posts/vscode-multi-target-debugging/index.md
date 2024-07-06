---
title: "如何在VSCode中同时调试多个进程"
date: 2024-07-06T20:53:30+08:00
draft: false
---

假设有`server.py`和`client.py`两个程序需要同时启动，只需要这么编写`.vscode/launch.json`即可：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
          "name": "Client",
          "type": "debugpy",
          "request": "launch",
          "program": "${workspaceFolder}/client.py"
        },
        {
          "name": "Server",
          "type": "debugpy",
          "request": "launch",
          "program": "${workspaceFolder}/server.py"
        }
    ],
    "compounds": [
        {
          "name": "Server/Client",
          "configurations": ["Server", "Client"],
          "stopAll": true
        }
    ]
}
```

接着运行名为`Server/Client`的启动项即可同时启动`Server`和`Client`两个调试任务。启用`stopAll`属性之后，一旦关闭正在调试的任何一个程序，就会将其他程序也一并关闭。
