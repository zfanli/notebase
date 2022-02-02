---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Concept: Tree-shaking

非兼容。

Vue 2.x 全局 API 无法经过 Webpack 等构筑工具将应用中为使用的代码移除（这个操作 Webpack 定义术语为 Tree-Shaking）。

在 Vue 3.x 中这些全局 API 不再挂载在全局对象上，而是需要具名导出使用。这样，所有未被导出过的 API 在构建过程中可以被移除，不进入最终打包的结果中，不增加应用的体积。

```js
// Vue 2.x
const plugin = {
  install: (Vue) => {
    Vue.nextTick(() => {
      // ...
    })
  },
}
```

```js
// Vue 3.x
import { nextTick } from "vue"

const plugin = {
  install: (app) => {
    nextTick(() => {
      // ...
    })
  },
}
```

## 受影响的 API

Vue 2.x 中的这些全局 API 受此更改的影响：

- `Vue.nextTick`
- `Vue.observable` (用 `Vue.reactive` 替换)
- `Vue.version`
- `Vue.compile` (仅完整构建版本)
- `Vue.set` (仅兼容构建版本)
- `Vue.delete` (仅兼容构建版本)
