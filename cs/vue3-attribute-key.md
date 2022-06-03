---
type: Vue
tags: Vue JavaScript QoK-B
created: 2022-02-01
---

# Attribute: key

非兼容。

## `v-if`/`v-else`/`v-else-if`

在 Vue 2.x 中建议在 `v-if` 系列指令中使用 `key` 属性标识节点的唯一身份，目的在于让 Vue 虚拟 DOM 知道何时可以重用和修复节点，以及何时需要重新排序和创建节点。

在 Vue 3.x 中不再建议在 `v-if` 系列指令中使用 `key` 属性，Vue 会自动计算一个唯一标识作为节点的身份。虽然 `key` 属性依然有效，但在使用它的时候需要人为保证唯一性。

## `<template v-for>`

在 Vue 2.x 中 `<template>` 标签不能设置 `key` 属性，你需要给 `<template>` 的子节点分别设置 `key` 属性。

在 Vue 3.x 中 `key` 属性应该直接给 `<template>` 设置。

```html
<!-- Vue 2.x -->
<template v-for="item in list">
  <div :key="'heading-' + item.id">...</div>
  <span :key="'content-' + item.id">...</span>
</template>

<!-- Vue 3.x -->
<template v-for="item in list" :key="item.id">
  <div>...</div>
  <span>...</span>
</template>
```

```html
<!-- Vue 2.x -->
<template v-for="item in list">
  <div v-if="item.isVisible" :key="item.id">...</div>
  <span v-else :key="item.id">...</span>
</template>

<!-- Vue 3.x -->
<template v-for="item in list" :key="item.id">
  <div v-if="item.isVisible">...</div>
  <span v-else>...</span>
</template>
```
