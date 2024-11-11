---
title: "关于CSRF攻击及其防御"
date: 2024-10-25T11:04:00+08:00
draft: false
---

CSRF攻击是利用了浏览器的一个安全漏洞。假设用户在a.com上登入，浏览器获得了cookie，接下来如果用户访问b.com，而b.com访问了a.com，则浏览器会将cookie附加在请求上，从而可能在用户不知情的情况下以登入权限进行了a.com的操作。尽管较新的浏览器修复了部分漏洞，但是由于要保持互联网的兼容性，因此并没有完全修复这个问题，并且我们不能假设用户使用的都是较新的浏览器，因此对CSRF攻击进行防御还是有必要的。

这里直接给出防御CSRF攻击的两种简单方法：

1. 不用浏览器的cookie机制进行认证。但是Cookie是浏览器原生支持的，并且Django自带的认证机制也是使用Cookie的，如果不用Cookie，就要造很多轮子。
2. 服务端要配置好CORS，并且API应该拒绝任何简单请求（simple request，其定义参见[这里](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests)），比如说，服务端可以只接受`Content-Type`为`application/json`的API请求。

这里有必要解释一下为什么单纯只设置CORS不能防御CSRF攻击。CORS机制对于简单请求和复杂请求的处理是不同的。对于复杂请求，它会先给服务器发送一个OPTION请求，如果服务器响应是允许，那么浏览器才会发起真正的请求。而对于简单请求，浏览器会**直接发送真正的请求**，只不过如果服务器的响应没有说允许请求，那么浏览器将不会允许脚本获得响应的值。此时服务器还是收到了请求。假如有一个API的功能是修改密码，那么只要服务器执行了该请求，攻击就已经成功了，这与浏览器脚本能否获得响应值无关。

参考资料：

1. https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

2. https://owasp.org/www-community/attacks/csrf

3. https://danielw.cn/web-security-xss-csrf-cn#csrf
