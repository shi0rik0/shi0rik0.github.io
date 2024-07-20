---
title: "关于VSCode的launch.json"
date: 2024-07-20T15:51:01+08:00
draft: false
---

VSCode工作区的`.vscode/launch.json`文件中存储着调试和运行的配置。

## 基本结构

`launch.json`的基本结构如下：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
          "name": "foo",
          "type": "debugpy",
          "request": "launch",
          "program": "${workspaceFolder}/bar.py"
        },
        // ...
    ]
}
```

首先有一个`version`字段指定配置文件的版本，然后在`configurations`数组中指定调试或运行的参数。

## 必须参数

每一个启动项都必须有这4个参数：

1. `name`：显示在VSCode界面上的启动项名称。
2. `type`：要使用的debugger。debugger都是由插件提供的。
3. `request`：只能是`launch`或者`attach`。`launch`表示调试时由VSCode启动一个新的进程，`attach`则表示让debugger连接到现有的进程。一般都是选择`launch`。
4. `program`：程序的入口文件。

## debugpy的参数

`cwd`参数可以指定运行脚本时的工作目录，默认值是`${workspaceFolder}`。

`justMyCode`参数的默认值是`true`，表示不调试不在当前工作区内的代码（比如标准库、第三方库），建议设置为`false`。

## 切换调试和运行模式

在VSCode里，调试和运行是共享配置的。按下F5会开始调试当前的选中项，按下Ctrl+F5则会以运行模式（不启动debugger）启动当前的选中项。

## 同时调试多个对象

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
