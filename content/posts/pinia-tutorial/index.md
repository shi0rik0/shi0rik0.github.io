---
title: 'Pinia快速入门'
date: 2025-01-12T15:47:59+08:00
draft: false
---

Pinia 是 Vue 3 官方的状态管理库，通俗点说就是可以给 Vue 应用程序提供一个“全局变量”。Pinia 的基本使用方法非常简单。

## 导入 Pinia

首先，我们需要安装 Pinia：

```bash
npm install pinia
```

然后在 Vue 3 项目中导入 Pinia：

```js
import { createApp } from 'vue'
import { createPinia } from 'pinia' // <--
import App from './App.vue'

const pinia = createPinia() // <--
const app = createApp(App)

app.use(pinia) // <--
app.mount('#app')
```

## 创建一个 Store

Store 是 Pinia 的一个核心概念，简单来说就是一组全局变量集合。我们可以通过 `defineStore` 函数来创建一个 Store：

```js
import { defineStore } from 'pinia'

export const useExampleStore = defineStore('example', {
  state: () => ({
    count: 0,
  }),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  actions: {
    increment() {
      this.count++
    },
  },
})
```

`defineStore`函数接受两个参数，第一个参数是 Store 的全局 ID，第二个参数是 Store 的配置。配置中包含了 `state`、`getters` 和 `actions` 三个属性，分别用来定义 Store 的状态、计算属性和方法。

## 使用 Store

在 Vue 组件中使用 Store 非常简单，只要 import 并调用 Store 的 use 函数即可：

```vue
<script setup>
import { useExampleStore } from '../stores/example'

const store = useExampleStore()
</script>

<template>
  <div>
    <p>Count: {{ store.count }}</p>
    <p>Double Count: {{ store.doubleCount }}</p>
    <button @click="store.increment">Increment</button>
  </div>
</template>
```

要注意，Store 对象本身就是响应式的，所以读取或修改状态的时候不需要使用`.value`:

```vue
<script setup>
import { useExampleStore } from '../stores/example'

const store = useExampleStore()

const increment = () => {
  // 错误的写法：store.count.value++
  store.count++
}
</script>

<template>
  <div>
    <p>Count: {{ store.count }}</p>
    <button @click="increment()">Increment</button>
  </div>
</template>
```

## 深入 state

在定义`state`的时候，Pinia 会自动推断类型，但是有时候我们需要手动指定类型：

```ts
defineStore('example', {
  state: () => ({
    names: [] as string[],
  }),
})
```

## 深入 getters

在 getter 中访问 state 的时候利用 state 参数（第一个参数）就可以了。如果希望在 getter 中使用其他 getter，可以使用`this`关键字，但是这时候就**不能**使用箭头函数了，并且这时候还需要手动指定返回值类型：

```ts
defineStore('example', {
  getters: {
    doubleCount: (state) => state.count * 2,
    quadrupleCount(state): number {
      return this.doubleCount * 2
    },
  },
})
```

## 深入 actions

一个 action 可以是异步函数。

在 action 中，可以直接访问其他 state、getter 和 action，并且都是通过`this`来访问，因此 action 也不能是箭头函数。
