---
type: Vue3
# tags: Vue3.x
created: 2022-02-01
---

# Directive: v-if & v-for

建议不要在一个标签同时使用 `v-if` 和 `v-for` 指令。可以考虑通过计算属性事先将列表计算出来，仅用 `v-for` 来渲染可见元素。

## 非兼容处理

如果同时使用了 `v-if` 和 `v-for`，Vue 3.x 存在非兼容处理。

- Vue 2.x 同时使用 `v-if` 和 `v-for` 时，`v-for` 会优先作用
- Vue 3.x 同时使用 `v-if` 和 `v-for` 时，`v-if` 会优先作用
