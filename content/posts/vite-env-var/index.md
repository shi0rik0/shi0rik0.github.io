---
title: "Vite配置开发/生产环境下的环境变量"
date: 2024-09-20T16:33:36+08:00
draft: false
---

在`vite.config.js`里面有一个`envDir`的选项，用来指定 Vite 加载的环境变量的路径，默认值是项目的根目录，可以改为其他值：

```js
export default defineConfig({
  envDir: "env",
});
```

在`envDir`的目录下新建三个文件：`.env`、`.env.development`和`.env.production`。其中`.env`是任何模式下都会加载的环境变量，`.env.development`只会在 development 模式下加载，而`.env.production`只会在 production 模式下加载。development 和 production 是 Vite 预定义的两个模式，你也可以定义自己的模式，比如 test，这样文件名就是`.env.test`。

development 是 Vite 在开发模式下的默认模式，而 production 是 Vite build 时的默认模式。也可以用`--mode`参数来手动设定模式。也就是说：

```bash
# 这两个命令是等价的
vite
vite --mode development

# 这两个命令是等价的
vite build
vite build --mode production
```

.env 文件的格式是简单的 key=value 的格式，其中 value 可以加引号，也可以不加，比如：

```
key1=value1
key2="value2"
```

为了防止机密信息泄露，Vite 默认只会导出以`VITE_`开头的变量。

要访问环境变量，可以在 JS 中使用`import.meta.env.VITE_XXX`来访问。需要注意，`import.meta`只能在 JS Module 的环境中使用，因此不能直接在 Vue 的 template 中访问，需要在 script 部分中先加载才行：

```vue
<script setup>
const XXX = import.meta.env.VITE_XXX;
</script>

<template>
  <p>{{ XXX }}</p>
</template>
```
