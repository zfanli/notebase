---
type: StateManagement
tags: StateManagement Vue JavaScript
---

# Pinia: Plugins

Pinia 的底层 API 被设计为接受完全扩展的形式，通过插件你可以达成：

- 给 [[vue-pinia-store|store]] 对象添加属性
- 在 store 定义时添加新的选项
- 给 store 对象添加新的方法
- 给现有的方法外面再进行一层包裹
- 修改或取消 [[vue-pinia-actions|action]]
- 实现本地储存功能
- 这些修改可以针对指定的 store 应用

插件使用 `pinia.use()` 方法注册。下面的例子通过插件的返回值给所有 store 添加一个新的静态属性。这个方法对需要添加一个全局对象管理路由、模态框活着轻提示框非常方便。

```js
import { createPinia } from 'pinia'

// add a property named `secret` to every store that is created after this plugin is installed
// this could be in a different file
function SecretPiniaPlugin() {
  return { secret: 'the cake is a lie' }
}

const pinia = createPinia()
// give the plugin to pinia
pinia.use(SecretPiniaPlugin)

// in another file
const store = useStore()
store.secret // 'the cake is a lie'
```

插件接受一个参数 `context`，返回值会被添加到 store 的属性上。**注意只有在注册了插件之后创建的 store 才会被影响**。

```js
export function myPiniaPlugin(context) {
  context.pinia // the pinia created with `createPinia()`
  context.app // the current app created with `createApp()` (Vue 3 only)
  context.store // the store the plugin is augmenting
  context.options // the options object defining the store passed to `defineStore()`
  // ...
}
```

通过插件给 state 添加新的属性时需要做两步：

1. 给 `store` 对象直接添加属性，保证这个值可以直接访问
2. 给 `store.$state` 对象添加属性，保证其可以被序列号

添加的属性可以是 `ref` 或者 `computed` 等响应式数据，这还能让你在不同 store 直接共享一份数据。

```js
const globalSecret = ref('secret')
pinia.use(({ store }) => {
  // `secret` is shared among all stores
  store.$state.secret = globalSecret
  store.secret = globalSecret
  // it gets automatically unwrapped
  store.secret // 'secret'

  const hasError = ref(false)
  store.$state.hasError = hasError
  // this one must always be set
  store.hasError = toRef(store.$state, 'hasError')

  // in this case it's better not to return `hasError` since it
  // will be displayed in the `state` section in the devtools
  // anyway and if we return it, devtools will display it twice.
})
```

注意在给 store 添加其他库的对象，或者任何非响应式数据时，需要使用 `markRaw()` 方法传递给 Pinia。

```js
import { markRaw } from 'vue'
// adapt this based on where your router is
import { router } from './router'

pinia.use(({ store }) => {
  store.router = markRaw(router)
})
```

[[vue-pinia-state|State]] 的 `$subscribe()` 和 [[vue-pinia-actions|action]] 的 `$onAction()` 可以在插件中使用。

```js
pinia.use(({ store }) => {
  store.$subscribe(() => {
    // react to store changes
  })
  store.$onAction(() => {
    // react to store actions
  })
})
```

TypeScript 支持。使用插件对 store 添加新的属性后需要对 TypeScript 的类型定义进行扩展，让编辑器可以自动补完新添加的属性。

```ts
import 'pinia'

declare module 'pinia' {
  export interface PiniaCustomProperties {
    // by using a setter we can allow both strings and refs
    set hello(value: string | Ref<string>)
    get hello(): string

    // you can define simpler values too
    simpleNumber: number
  }
}
```

添加新的选项类型定义。

```ts
import 'pinia'

declare module 'pinia' {
  export interface DefineStoreOptionsBase<S, Store> {
    // allow defining a number of ms for any of the actions
    debounce?: Partial<Record<keyof StoreActions<Store>, number>>
  }
}
```
