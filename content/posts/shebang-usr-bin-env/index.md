---
title: "在shebang中使用/usr/bin/env"
date: 2024-05-11T17:12:11+08:00
draft: false
---

假设我们要编写一个bash脚本，很多人会这么写shebang：
```
#!/bin/bash
```
这么做相当于是指定了绝对路径，但有时候，我们希望操作系统在PATH中寻找程序，这时候就可以利用`/usr/bin/env`：
```
#!/usr/bin/env bash
```

