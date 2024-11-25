---
title: "关于npm install的开发依赖"
date: 2024-11-25T19:48:00+08:00
draft: false
---

`npm`在管理依赖的时候，会将依赖分为生产依赖和开发依赖。生产依赖就是运行的时候用到的依赖，而开发依赖则是测试框架、linter 等开发时才会用到的辅助工具。

如果你用`npm install {package}`，那么默认就是作为生产依赖去安装。要安装一个开发依赖，就要指定`--save-dev`或者`-D`：

```sh
npm install --save-dev {package}
# 或者
npm install -D {package}
```

（有的教程会说，安装生产依赖的时候要指定`--save`，也就是：`npm install --save {package}`，这是过时的说法，新版本`npm`已经将此作为默认值了。）

在部署的时候，如果不希望安装开发依赖，那么可以用`--omit=dev`参数：

```sh
npm install --omit=dev
```

（有的教程会说用`--production`参数，但是新版本`npm`已经不推荐使用这个了，如果用这个，会得到一个 warning。）

（吐槽时间：`npm`怎么老是随意更改接口。）
