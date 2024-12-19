---
title: "关于clangd的配置文件"
date: 2024-12-18T19:27:00+08:00
draft: false
---

clangd 是一个可以分析 C++代码，并帮助编辑器进行代码补全、跳转等功能的工具，但是 clangd 不会自动分析项目的 include 路径、编译选项等信息，这些信息需要通过配置文件来提供。clangd 支持两种配置文件，第一种是`compile_commands.json`，这个文件会给项目中的每一个源文件分别指定编译选项，通常这个文件是由程序自动生成的。第二种则是`compile_flags.txt`，这个文件会给项目中的所有文件指定相同的编译选项，这个文件往往是手动编写的。

## 配置文件的查找路径

clangd 的文档写得不是很清楚，根据我所了解的信息，clangd 查找配置文件的逻辑大概是这样的：

如果使用的是`compile_flags.txt`，那么 clangd 只会在项目根目录下查找这个文件。如果使用的是`compile_commands.json`，那么 clangd 会从当前编辑的源文件所在的目录开始，逐级向上查找`compile_commands.json`，并且途中也会在每一个目录的`build/`目录下查找。举个例子，假如当前编辑的文件是`/app/src/foo.cpp`，那么 clangd 会依次在以下目录查找：`/app/src/`、`/app/src/build/`、`/app/`、`/app/build/`……。目前似乎没有办法手动指定配置文件的路径。

## compile_flags.txt 的格式

`compile_flags.txt`的格式非常简单，只需要在每一行写一个`clang`的编译选项即可。例如：

```txt
-I/usr/include
-std=c++17
-Wall
-Wextra
```

## 使用 CMake 生成 compile_commands.json

要使用 CMake 生成`compile_commands.json`，只需要在`CMakeLists.txt`中启用该功能即可：

```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```

这样在构建项目的时候，CMake 就会在生成目录（通常是`build/`）下面生成`compile_commands.json`文件。

## 使用 Bear 生成 compile_commands.json

对于使用传统的 Makefile 构建的项目，可以使用[Bear 工具](https://github.com/rizsotto/Bear)来生成`compile_commands.json`。在 Ubuntu 下，Bear 可以通过 APT 安装，包名为`bear`。
