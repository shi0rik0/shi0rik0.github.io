---
title: "Windows家庭版关闭BitLocker"
date: 2024-07-20T12:09:30+08:00
draft: false
---

现在很多笔记本出厂时会默认启用 BitLocker。通过运行`manage-bde -status`命令可以查看分区是否有启用 BitLocker（这个命令需要管理员权限）。

如果是家庭版的 Windows（可以通过运行`winver`确认），那么 BitLocker 可以用系统设置的“隐私和安全性”->“设备加密”进行设置。这个“设备加密”就是家庭版 Windows 自带的阉割版 BitLocker。关闭“设备加密”就可以解密了。解密完成后，可以通过运行`manage-bde -status`命令确认是否已经解密成功。
