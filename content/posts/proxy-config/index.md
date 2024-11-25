---
title: "代理服务器设置方法"
date: 2024-03-31T15:28:42+08:00
draft: false
---

由于 GFW 的存在，在中国境内访问境外的网站有时候会非常地慢。使用代理可以解决问题，但是有些程序没有使用系统全局的代理设置，需要自己手动配置代理。本文总结了一些需要手动配置的程序的配置方法。

## npm

```
npm config set proxy http://proxy:port
npm config set https-proxy http://proxy:port
```
