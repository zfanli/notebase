---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# App API: createApp

非兼容。

Vue 3.x 加入了 App 的概念。全局 API `createApp` 返回一个 App 实例。

```js
import { createApp } from "vue"

const app = createApp({})
```

CDN 版本的全局 API 从 `Vue` 对象暴露。

```js
const { createApp } = Vue

const app = createApp({})
```

## App 实例 API

App 实例暴露了 Vue 2.x API 的子集。下面是 Vue 2.x 全局 API 对应到 Vue 3.x App 实例 API 的列表。

| 2.x 全局 API               | 3.x 实例 API (app)                                  |
| -------------------------- | --------------------------------------------------- |
| Vue.config                 | app.config                                          |
| Vue.config.productionTip   | 移除                                                |
| Vue.config.ignoredElements | app.config.compilerOptions.isCustomElement (见下方) |
| Vue.component              | app.component                                       |
| Vue.directive              | app.directive                                       |
| Vue.mixin                  | app.mixin                                           |
| Vue.use                    | app.use (见下方)                                    |
| Vue.prototype              | app.config.globalProperties (见下方)                |
| Vue.extend                 | 移除 (见下方)                                       |

Vue 3.x App 初始化示例。

```js
const app = createApp(MyApp)

app.component("button-counter", {
  data: () => ({
    count: 0,
  }),
  template: '<button @click="count++">Clicked {{ count }} times.</button>',
})

app.directive("focus", {
  mounted: (el) => el.focus(),
})

// 现在，所有通过 app.mount() 挂载的应用实例及其组件树，
// 将具有相同的 “button-counter” 组件和 “focus” 指令，
// 而不会污染全局环境
app.mount("#app")
```

### `config.ignoredElements` 替换为 `config.isCustomElement`

用来过滤原生 HTML 元素。

```js
// 之前
Vue.config.ignoredElements = ["my-el", /^ion-/]

// 之后
const app = createApp({})
app.config.compilerOptions.isCustomElement = (tag) => tag.startsWith("ion-")
```

### `Vue.prototype` 替换为 `config.globalProperties`

```js
// 之前 - Vue 2
Vue.prototype.$http = () => {}
```

```js
// 之后 - Vue 3
const app = createApp({})
app.config.globalProperties.$http = () => {}
```

### `app.use`

```js
// 之前 - Vue 2
Vue.use(VueRouter)

// 之后 - Vue 3
const app = createApp(MyApp)
app.use(VueRouter)
```

## 参考

- [全局 API | Vue.js](https://v3.cn.vuejs.org/guide/migration/global-api.html#%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84%E5%85%A8%E5%B1%80-api-createapp)
