---
type: Vue
tags: Vue JavaScript QoK-B
created: 2022-02-01
---

# Directive: v-for

## ref 不再注册数组

非兼容。

Vue 2.x 中，`v-for` 中使用 `ref` 属性会自动注册元素实例数组到 `$refs` 原型属性中。当 `v-for` 存在嵌套时这种行为变得低效且不明确。

Vue 3.x 中 `v-for` 将不会注册 `ref` 数组，你可以通过一个函数来单独处理每一个 `ref` 元素实例。

```html
<div v-for="item in list" :ref="setItemRef"></div>
```

选项 API。

```js
export default {
  data() {
    return {
      itemRefs: [],
    }
  },
  methods: {
    setItemRef(el) {
      if (el) {
        this.itemRefs.push(el)
      }
    },
  },
  beforeUpdate() {
    this.itemRefs = []
  },
  updated() {
    console.log(this.itemRefs)
  },
}
```

组合式 API。

```js
import { onBeforeUpdate, onUpdated } from 'vue'

export default {
  setup() {
    let itemRefs = []
    const setItemRef = (el) => {
      if (el) {
        itemRefs.push(el)
      }
    }
    onBeforeUpdate(() => {
      itemRefs = []
    })
    onUpdated(() => {
      console.log(itemRefs)
    })
    return {
      setItemRef,
    }
  },
}
```

注意：

- `itemRefs` 不必是数组：它也可以是一个对象，其 `ref` 可以通过迭代的 `key` 被设置。
- 如有需要，`itemRefs` 也可以是响应式的，且可以被侦听。
