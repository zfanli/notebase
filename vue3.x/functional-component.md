---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Functional Component 函数式组件

非兼容。

- `<template>` 的 `functional` 属性被移除
- `{ functional: true }` 选项被移除

Vue 2.x 中函数式组件的使用场景为

- 性能优化，函数式组件的初始化速度要比有状态组件快
- 返回多个根节点

但是在 Vue 3.x 中

- 函数式组件和有状态组件初始化的性能差异可以忽略不计
- 有状态组件支持多个根节点

所以官方建议全都使用有状态组件。
