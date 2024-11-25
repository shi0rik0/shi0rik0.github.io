---
title: "PowerShell脚本被禁止执行的解决方法"
date: 2024-04-09T17:14:38+08:00
draft: false
---

在 Windows 中执行 PowerShell 脚本的时候，默认情况下会得到下面的错误信息：

> 无法加载文件 xxx.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。

这是因为默认情况下，Windows 的 PowerShell 脚本执行策略是`Restricted`，也就是阻止运行任何脚本。要允许执行任意脚本，只需要将执行策略修改为`Unrestricted`即可：

```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
```
