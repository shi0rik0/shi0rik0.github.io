---
title: "关于C++编译器的警告开关"
date: 2024-12-28T22:15:00+08:00
draft: false
---

C++编译器（主要是 GCC 和 Clang）都会提供一些控制警告的编译选项，这些选项的格式通常是`-W<name>`或者`-Wno-<name>`，其中`<name>`是警告的名称。例如`-Wuninitialized`表示启用“未初始化变量”的警告，`-Wno-unused-variable`表示禁用“未使用变量”的警告。

由于警告有很多种类，逐一启用会非常麻烦，因此编译器也提供了一些选项来批量启动警报，其中一个很有误导性的选项是`-Wall`。很多人会误以为这个选项会启用所有警告，但实际上这只会启用一部分警报。如果想要启用更多警告，可以再添加`-Wextra`选项。但是就算使用`-Wall -Wextra`也仍然不会启用所有警告。

如果你有洁癖，想要启用所有警告，可以使用`-Weverything`选项，但是这个选项只有 Clang 支持，GCC 不支持。并且，由于有些警告过于严格或者迂腐了，所以启用所有警告或许并不是一个好主意。

我个人的习惯是使用`-Wall -Wextra`，我觉得这是一个比较折中的选择，并且 Clang 和 GCC 都支持这些选项。

## 参考资料

https://embeddedartistry.com/blog/2017/06/07/warnings-weverything-and-the-kitchen-sink/
