---
title: "用Makefile编写生成多个目标文件的规则"
date: 2024-04-27T11:42:02+08:00
draft: false
---

假设现在有一个命令 cmd，它会读取文件 a，然后生成文件 x 和 y。

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    puts("Reading a.");
    system("cat a");
    puts("Creating x and y.");
    system("touch x y");
    return 0;
}
```

我们可能很自然地会写出这样的 Makefile 规则：

```
all: x y ;

x y: a
    cmd
```

乍一看，这似乎没问题，但是当我们并发运行 make 的时候，问题就出现了。假设我们运行`make -j2`，就会看到以下输出：

```
Reading a.
Reading a.
Creating x and y.
Creating x and y.
```

也就是说，这个命令被执行了两次！这是因为，当我们编写

```
x y: a
    cmd
```

的时候，实际上等价于

```
x: a
    cmd

y: a
    cmd
```

这样一来，当多线程运行的时候，make 就会并行地同时生成 x 和 y，导致重复执行。

后来，我在[这个 StackOverflow 问题](https://stackoverflow.com/questions/2973445/gnu-makefile-rule-generating-a-few-targets-from-a-single-source-file)中找到了解决办法。

## GNU Make v4.3 之后的版本

GNU Make v4.3 新增了 grouped explicit targets 的特性，我们只需要在冒号前面加一个&就可以解决问题。

```
x y &: a
    cmd
```

## GNU Make v4.3 之前的版本

如果需要考虑到版本兼容性，那么还有一种更通用但是更复杂的解决方法，这种方法利用了一个虚拟的中间文件 xy.intermediate。

```
x y: xy.intermediate ;

.INTERMEDIATE: xy.intermediate
xy.intermediate: a
	cmd
```

为了让这个方法用起来更加方便，我编写了一个 VSCode snippet：

```json
"Multiple outputs": {
    "prefix": ["multiout"],
    "body": ["${1:<TARGETS>}: ${1/[^0-9a-zA-Z_\\-\\.]//g}.intermediate ;",
                "",
                ".INTERMEDIATE: ${1/[^0-9a-zA-Z_\\-\\.]//g}.intermediate",
                "${1/[^0-9a-zA-Z_\\-\\.]//g}.intermediate: ${2:<DEPS>}",
                "\t"],
    "description": "A pattern for recipes with multiple outputs."
}
```
