---
title: "在Vite项目中使用Tailwind CSS"
date: 2024-12-19T20:51:03+08:00
draft: false
---

本文介绍如何在现有的 Vite 项目中使用 Tailwind CSS。

首先要安装 Tailwind CSS：

```bash
npm install -D tailwindcss postcss autoprefixer
```

然后初始化配置文件：

```bash
npx tailwindcss init -p
```

这将生成两个文件：`tailwind.config.js`和`postcss.config.js`。

接下来在`tailwind.config.js`中配置`content`选项。只有`content`中指定的文件才可以使用 Tailwind CSS。例如：

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,jsx,ts,tsx,vue}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

要让 Tailwind CSS 生效，还需要让网页导入 Tailwind CSS 的样式。比如说，可以在全局的 CSS 文件中加入以下内容：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

到这里就配置完成了。我们不需要修改 Vite 的配置文件，因为 Tailwind CSS 是基于 PostCSS 框架的，而 Vite 会自动检测并启用 PostCSS 插件。
