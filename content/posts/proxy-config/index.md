---
title: "代理服务器设置方法"
date: 2025-01-06T20:28:42+08:00
draft: false
---

由于 GFW 的存在，在中国境内访问境外的网站有时候会非常的慢，这时候就只能使用代理，但是有些程序没有使用系统全局的代理设置，需要自己手动配置代理。本文总结了一些需要手动配置的程序的配置方法。

## git

Git 没有专门针对 HTTPS 协议的代理设置，不管是 HTTP 还是 HTTPS，都是使用 `http.proxy` 来设置代理。

```bash
git config --global http.proxy # 查看代理设置
git config --global http.proxy http://example.com:8000
git config --global --unset http.proxy
```

## npm

```bash
npm config get proxy
npm config get https-proxy
npm config set proxy http://example.com:8000
npm config set https-proxy http://example.com:8000
npm config delete proxy
npm config delete https-proxy
```
