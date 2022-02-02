---
type: Vue
tags: Vue3.x
created: 2022-02-02
---

# Option: setup

新增。组合式 API。

## 参数

1. `props` 响应式
2. `context` 非响应式

### `props`

第一个参数 `props` 是响应式的，这意味着它无法进行解构赋值。

Vue 3.x 提供一个 [[global-api-toRefs|全局 API toRefs]] 使其可以被解构赋值。解构后的响应式数据结构和 [[global-api-ref|全局 API ref]] 一致。

```js
// MyBook.vue

import { toRefs } from 'vue'

setup(props) {
  const { title } = toRefs(props)

  console.log(title.value)
}
```

上面的用法前提是 `title` 是必须的 props，如果它是可选的，那么在其不存在时 `toRefs` 不会为其创建一个新的响应式变量。这时可以使用 [[global-api-toRef|全局 API toRef]] 来在可选值未传值的情况也返回一个响应式变量。

```js
// MyBook.vue
import { toRef } from 'vue'
setup(props) {
  const title = toRef(props, 'title')
  console.log(title.value)
}
```

### `context`

第二个参数 `context` 提供了一些 `setup` 中可能有用的值。

其中 `expose` 是一个函数，接受一个对象作为参数，其作用和 `setup` 返回对象的效果一致，可以将参数对象的 property 暴露给模版和其他实例选项中访问。这个函数存在的意义在于 `setup` 返回渲染函数时，提供一个方式让其依然能够暴露其他实例选项需要访问的 property。详细见下文。

```js
// MyBook.vue
export default {
  setup(props, context) {
    // Attribute (非响应式对象，等同于 $attrs)
    console.log(context.attrs)

    // 插槽 (非响应式对象，等同于 $slots)
    console.log(context.slots)

    // 触发事件 (方法，等同于 $emit)
    console.log(context.emit)

    // 暴露公共 property (函数)
    console.log(context.expose)
  },
}
```

这个参数是非响应式的，可以安全解构。

```js
// MyBook.vue
export default {
  // 可以安全解构
  setup(props, { attrs, slots, emit, expose }) {
    ...
  }
}
```

**但是 `attrs` 和 `slots` 是有状态的，应该避免对其进行解构，始终使用 `attrs.x` 或 `slots.x` 的方式引用其属性。** 这俩个属性和 `props` 不同，它们是非响应式的，你需要在 [[global-api-onBeforeUpdate|onBeforeUpdate]] 生命周期钩子中对其的变更进行处理。

## 结合模版使用

如果 `setup` 返回一个对象，这个对象所有的 property 和传递给 `setup` 的 `props` 的所有 property 就都能在模版中访问。

> 注意通过全局 API 创建的 [[concept-refs|refs]] 会被自动浅解包（即仅解包一层，如果有嵌套 refs，则还是需要通过 `.value` 访问嵌套属性的值。

```vue
<!-- MyBook.vue -->
<template>
  <div>{{ collectionName }}: {{ readersNumber }} {{ book.title }}</div>
</template>

<script>
import { ref, reactive } from "vue"

export default {
  props: {
    collectionName: String,
  },
  setup(props) {
    const readersNumber = ref(0)
    const book = reactive({ title: "Vue 3 Guide" })

    // 暴露给 template
    return {
      readersNumber,
      book,
    }
  },
}
</script>
```

## 使用渲染函数

`setup` 选项除了返回对象之外，还可以返回一个渲染函数。此时会让 `setup` 没法暴露属性给其他选项访问，`expose` 可以起到和返回一个对象一样的效果。

```js
import { h, ref } from "vue"
export default {
  setup(props, { expose }) {
    const count = ref(0)
    const increment = () => ++count.value

    expose({
      increment,
    })

    return () => h("div", count.value)
  },
}
```

## `this` 在 `setup` 中的行为

`setup` 会在解析其他选项之前执行，所以在 `setup` 中 `this` 不会指向组件的实例。这在和选项式 API 混用时可能会造成混淆。
