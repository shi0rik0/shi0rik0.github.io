---
title: "给VSCode Vue项目配置ESLint和Prettier"
date: 2024-11-25T16:19:00+08:00
draft: false
---

## 什么是 ESLint 和 Prettier

ESLint 是一个专门针对 JS 程序的 linter，但是它也可以用来格式化 JS 代码。而 Prettier 是一个支持多种语言（包括 JS）的代码 formatter。Linter 和 formatter 的区别在于，formatter 只关注代码格式好不好看，不关心具体逻辑，而 linter 更关注程序逻辑是否有问题。

所以说，对于 HTML 和 CSS 代码，只能用 Prettier 来格式化。而对于 JS 代码来说，ESLint 和 Prettier 的功能有重复的部分。比较好的做法是，禁用 ESLint 中与 Prettier 冲突的格式化规则，然后同时使用 ESLint 和 Prettier。

## 安装并配置 Prettier

在下文中，我们假设项目的目录结构是这样的：

```
.
├── backend/
├── frontend/
│   ├── node_modules/
│   ├── package.json
│   ├── package-lock.json
```

首先我们要安装 Prettier：

```sh
npm install -D prettier
```

然后在`frontend/`下面创建一个名为`.prettierrc`的 Prettier 配置文件。

```json
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

然后就可以用`npx`运行 Prettier 了：

```sh
npx prettier --check "**/*.js"
```

## 安装并配置 ESLint

首先安装依赖：

```sh
npm install -D eslint eslint-config-prettier eslint-plugin-vue
```

其中`eslint`是 ESLint 本体，`eslint-config-prettier`是一个配置文件，用来禁用 ESLint 中与 Prettier 冲突的规则，`eslint-plugin-vue`则用来给 ESLint 添加 Vue 相关的规则。

然后在`frontend/`下面创建名为`eslint.config.js`的 ESLint 配置文件：

```js
import js from "@eslint/js";
import prettier from "eslint-config-prettier";
import pluginVue from "eslint-plugin-vue";

export default [
  js.configs.recommended, // 使用推荐的 ESLint JS 配置
  ...pluginVue.configs["flat/recommended"], // 使用推荐的 ESLint Vue 配置
  prettier, // 这个配置会禁用和 Prettier 有冲突的 ESLint 规则
  {
    rules: {
      // 在这里写其他的配置
      // 参考：https://eslint.org/docs/latest/use/configure/configuration-files
    },
  },
];
```

接着就可以使用 ESLint 了：

```sh
npx eslint "**/*.js"
```

## 配置 VSCode

首先安装以下 VSCode 插件：

- dbaeumer.vscode-eslint
- esbenp.prettier-vscode
- Vue.volar

然后打开工作区的配置文件`.vscode/settings.json`，在里面加入以下内容：

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },

  "prettier.configPath": "./frontend/.prettierrc",
  "eslint.workingDirectories": ["./frontend/"],

  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```
