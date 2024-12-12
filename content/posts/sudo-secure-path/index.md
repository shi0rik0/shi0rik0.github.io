---
title: "关于sudo的secure_path设置"
date: 2024-12-11T21:48:00+08:00
draft: false
---

sudo 有一个名为`secure_path`的设置，从 sudo 1.9.16 开始，`secure_path`设置会默认启用。[官方博客](https://www.sudo.ws/posts/2024/09/why-sudo-1.9.16-enables-secure_path-by-default/)介绍了这个改动的原因。

当使用 sudo 执行命令的时候，用户当前的环境变量会被修改。其中`PATH`会被设置为`secure_path`的值。因此，经常会发生加了`sudo`之后就找不到命令的情况。解决方法也是非常的简单，要么修改`secure_path`的值，要么直接删除这个设置，这样 sudo 就不会修改`PATH`了。

运行`sudo visudo`，然后删除或者修改下面这一行即可。

```
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

还有一种方法是使用绝对路径来运行命令，可以用`which`命令来查看命令的绝对路径。下面是一个例子。

```bash
sudo $(which COMMAND)
```
