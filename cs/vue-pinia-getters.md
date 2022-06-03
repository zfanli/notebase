---
type: StateManagement
tags: StateManagement Vue JavaScript QoK-B
---

# Pinia: Getters

Getter 就是 [[vue-pinia-store|store]] 的计算属性。Getter 函数接受一个参数 state 来访问数据，这个设计旨在推荐使用箭头函数来定义 getter。不过 getter 函数有时需要引用其他 getter 来完成计算，这时通过定义普通函数并使用 `this` 关键字可以访问到整个 store 实例。需要注意的是 `this` 会导致类型推断失效，在配合 TypeScript 使用时需要显示定义函数的返回值类型。

```js
// 箭头函数
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  getters: {
    doubleCount: (state) => state.counter * 2,
  },
})

// 普通函数
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  getters: {
    // automatically infers the return type as a number
    doubleCount(state) {
      return state.counter * 2
    },
    // the return type **must** be explicitly set
    doublePlusOne(): number {
      // autocompletion and typings for the whole store ✨
      return this.doubleCount + 1
    },
  },
})
```

Getter 可以直接通过 store 实例使用。

```html
<template>
  <p>Double count is {{ store.doubleCount }}</p>
</template>

<script>
  export default {
    setup() {
      const store = useStore()

      return { store }
    },
  }
</script>
```

Getter 中使用 getter，上面说到 `this` 关键字会破坏类型推测，需要显式定义函数返回值类型，如果你没有使用 TypeScript，可以通过 JSDoc 指定。

```js
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  getters: {
    // type is automatically inferred because we are not using `this`
    doubleCount: (state) => state.counter * 2,
    // here we need to add the type ourselves (using JSDoc in JS). We can also
    // use this to document the getter
    /**
     * Returns the counter value times two plus one.
     *
     * @returns {number}
     */
    doubleCountPlusOne() {
      // autocompletion ✨
      return this.doubleCount + 1
    },
  },
})
```

Getter 本身只是一个属性，不接受参数。但是你可以通过 getter 返回函数来接受参数计算结果。需要注意这会破坏 getter 的缓存特性，getter 函数相当于一个普通的方法。但是在返回方法之前定义的变量还是可以缓存的，你可以通过缓存局部变量来做一些优化。

```js
export const useStore = defineStore('main', {
  getters: {
    getUserById: (state) => {
      return (userId) => state.users.find((user) => user.id === userId)
    },
  },
})

// 局部缓存
export const useStore = defineStore('main', {
  getters: {
    getActiveUserById(state) {
      // 这个变量会被缓存
      const activeUsers = state.users.filter((user) => user.active)
      return (userId) => activeUsers.find((user) => user.id === userId)
    },
  },
})
```

在组件中使用 getter 函数。

```html
<script>
  export default {
    setup() {
      const store = useStore()

      return { getUserById: store.getUserById }
    },
  }
</script>

<template>
  <p>User 2: {{ getUserById(2) }}</p>
</template>
```

使用其他 store 的 getter，直接导入并使用。

```js
import { useOtherStore } from './other-store'

export const useStore = defineStore('main', {
  state: () => ({
    // ...
  }),
  getters: {
    otherGetter(state) {
      const otherStore = useOtherStore()
      return state.localData + otherStore.data
    },
  },
})
```
