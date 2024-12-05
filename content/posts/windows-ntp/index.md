---
title: "Windows NTP指南"
date: 2024-12-05T9:01:00+08:00
draft: false
---

本文将介绍如何在 Windows 系统上启动 NTP 服务器，以及如何让 NTP 客户端同步时间。

注意：本文中的大部分操作都需要管理员权限。

## 防火墙配置

默认配置下，Windows 防火墙会阻止所有未经允许的入站连接。因此，我们需要允许 NTP 服务（使用 UDP 123 端口）的入站连接。

```powershell
New-NetFirewallRule -DisplayName "Allow NTP" -Direction Inbound -Protocol UDP -LocalPort 123 -Action Allow
```

## 启动 NTP 服务器

默认配置下，Windows 的 NTP 服务（`w32time`）是禁用服务器功能的，我们需要手动修改注册表，然后重启服务来开启：

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer" -Name "Enabled" -Value 1
Restart-Service w32time
```

然后运行下面的命令让`w23time`开机自动启动，并查看其状态：

```powershell
Set-Service w32time -StartupType Automatic
Get-Service w32time | Select-Object -Property Name, StartType, Status
```

接下来查看`w32time`的配置：

```powershell
w32tm /query /configuration
```

应该会在输出的`[TimeProviders]`部分看到类似下面的内容：

```
NtpServer (本地)
DllName: C:\WINDOWS\system32\w32time.dll (本地)
Enabled: 1 (本地)
```

## 向 NTP 服务器同步时间

首先启动`w32time`服务，并让它开机自动启动：

```powershell
Start-Service w32time
Set-Service w32time -StartupType Automatic
Get-Service w32time | Select-Object -Property Name, StartType, Status
```

然后运行下面的命令配置 NTP 客户端要向哪个服务器同步时间：

```powershell
$ntpServer = "127.0.0.1"
w32tm /config /manualpeerlist:$ntpServer /syncfromflags:manual /reliable:yes /update
```

最后运行下面的命令来强制 NTP 客户端立即同步时间：

```powershell
Set-Date (Get-Date).AddSeconds(-5) # 将本地时间提前5秒
w32tm /resync /force
```

这里要将本地时间提前 5 秒是为了确保`w32time`立即同步时间。经过测试，`w32time`的逻辑是如果时间相差不多就不会立即同步，所以我们需要故意将本地时间弄得不准，这样才能强制同步。

可以通过下面的命令来检验同步结果：

```powershell
w32tm /stripchart /computer:$ntpServer /samples:5
```

所看到的输出应该是类似下面这样的：

```
09:16:23, d:+00.0004502s o:+00.0001461s  [                           *                           ]
```

其中`o:`代表 offset，也就是本地时间和 NTP 服务器时间的差值（当然这是个估计值）。如果这个值较低，那么就说明同步成功了。

## 关于 Windows NTP 实现的精度

根据 Windows 官方文档，在 Windows 8 之前，Windows 的 NTP 实现并不标准，因此精度很差，只能达到秒的量级。而在 Windows 10 之后，微软改进了 NTP 服务的实现，现在的精度可以达到毫秒级别。具体可以参考以下文档：

1. [Configuring systems for high accuracy](https://learn.microsoft.com/en-us/windows-server/networking/windows-time-service/configuring-systems-for-high-accuracy)
2. [Support boundary for high-accuracy time](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/support-boundary-high-accuracy-time)（这个文档详细描述了达到不同精度的环境要求。）
