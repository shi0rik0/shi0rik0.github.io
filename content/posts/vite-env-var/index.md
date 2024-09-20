---
title: "Vite配置开发/生产环境下的环境变量"
date: 2024-09-20T16:33:36+08:00
draft: false
---

在`vite.config.js`里面有一个`envDir`的选项，用来指定Vite加载的环境变量的路径，默认值是项目的根目录，可以改为其他值：
```js
export default defineConfig({
  envDir: "env"
})
```

在`envDir`的目录下新建三个文件：`.env`、`.env.development`和`.env.production`。其中`.env`是任何模式下都会加载的环境变量，`.env.development`只会在development模式下加载，而`.env.production`只会在production模式下加载。development和production是Vite预定义的两个模式，你也可以定义自己的模式，比如test，这样文件名就是`.env.test`。

development是Vite在开发模式下的默认模式，而production是Vite build时的默认模式。也可以用`--mode`参数来手动设定模式。也就是说：
```bash
# 这两个命令是等价的
vite
vite --mode development

# 这两个命令是等价的
vite build
vite build --mode production
```

.env文件的格式是简单的key=value的格式，其中value可以加引号，也可以不加，比如：
```
key1=value1
key2="value2"
```

为了防止机密信息泄露，Vite默认只会导出以`VITE_`开头的变量。

要访问环境变量，可以在JS中使用`import.meta.env.VITE_XXX`来访问。需要注意，`import.meta`只能在JS Module的环境中使用，因此不能直接在Vue的template中访问，需要在script部分中先加载才行：
```vue
<script setup>
const XXX = import.meta.env.VITE_XXX
</script>

<template>
    <p>{{ XXX }}</p>
</template>
```

