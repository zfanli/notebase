---
type: StateManagement
tags: StateManagement Vue JavaScript
---

# Pinia: Store

在 Pinia 中通过 `defineStore()` API 来创建 store，第一个参数需要是一个唯一的名称，通常用 id 指代，Pinia 用它来将 store 关联到开发工具上。使用 `use...` 开头的名称命名用于组合式编程的函数是一种惯例，使其符合使用习惯。

```js
import { defineStore } from 'pinia'

// 第一个参数为这个 store 的 id
export const useState = defineStore('main', {
  // other options...
})
```

## 使用 Store

`defineStore()` 函数正如其名，我们只是定义了一个 Store，但是还未使用它。在组合式 API 的 `setup` 选项中调用上面定义的 `useState` 即可使用 Store。

```js
import { useStore } from '@/stores/counter'

export default {
  setup() {
    const store = useStore()

    return {
      // you can return the whole store instance to use it in the template
      store,
    }
  },
}
```

Pinia 建议使用不同的文件来定义 Store，这样在使用代码分割或 TypeScript 时更加方便。除了组合式 API 之外，Pinia 还提供和 Vuex 的 map helpers API 类似的函数让你可以像在 Vue 2.x 中使用 Vuex 同样的方式使用 Pinia。

`store` 对象是由 [[vue3-global-api-reactive|reactive]] 函数创建的对象，你不需要使用 `.value` 访问，同时你也不能解构它。

Pinia 提供 `storeToRefs()` API 帮助解构 Store。
