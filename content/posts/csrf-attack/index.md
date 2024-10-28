---
title: "关于CSRF攻击及其防御"
date: 2024-10-25T11:04:00+08:00
draft: false
---

CSRF攻击是利用了浏览器的一个安全漏洞。假设用户在a.com上登入，浏览器获得了cookie，接下来如果用户访问b.com，而b.com访问了a.com，则浏览器会将cookie附加在请求上，从而可能在用户不知情的情况下以登入权限进行了a.com的操作。尽管较新的浏览器修复了部分漏洞，但是由于要保持互联网的兼容性，因此并没有完全修复这个问题，并且我们不能假设用户使用的都是较新的浏览器，因此对CSRF攻击进行防御还是有必要的。

这里直接给出防御CSRF攻击的一些简单方法：要么不用浏览器的cookie机制进行认证。要么服务端要配置好CORS，并且API应该拒绝任何simple request（其定义参见[这里](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests)），比如说，服务端可以只接受`Content-Type`为`application/json`的API请求。

参考资料：

1. https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

2. https://owasp.org/www-community/attacks/csrf

3. https://danielw.cn/web-security-xss-csrf-cn#csrf
