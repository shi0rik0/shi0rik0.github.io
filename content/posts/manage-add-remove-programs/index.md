---
title: "管理Windows“添加或删除程序”中的项目"
date: 2024-05-15T20:45:17+08:00
draft: false
---

在Windows的控制面板中点击“添加或删除程序”，便会看到当前电脑中安装的程序。这些程序的信息实际上是存储在注册表中的`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`。这个注册表项下的每一个子项都代表了一个安装的程序。每个项记录的信息可以参见[微软的官方文档](https://learn.microsoft.com/en-us/windows/win32/msi/uninstall-registry-key)。下面简要介绍关键的信息。下面提到的值类型都是字符串。

- DisplayName: 程序的名称。
- DisplayIcon: 程序的图标。指定一个exe文件的路径，就会以这个exe文件的图标作为程序的图标。
- DisplayVersion: 程序的版本。
- Publisher: 程序的发行商。
- InstallDate: 程序的安装日期。格式是YYYYMMDD，例如20081225。
- UninstallString: 程序的卸载方式。在这里指定一个exe文件的路径，则卸载的时候会执行这个exe文件。
