---
type: Vue
tags: Vue JavaScript QoK-B
---

# Vue 3.x 迁移指南

## 非兼容变更

### 全局 API

- [[vue3-global-api-createApp|createApp]]
- [[vue3-concept-tree-shaking|全局 API 支持 Tree-Shaking]]

### 模版指令

- [[vue3-directive-v-model|Directive: v-model]]
- [[vue3-attribute-key|Attribute: key]]
- [[vue3-directive-v-if-and-v-for|Directive: v-if & v-for]]
- [[vue3-directive-v-bind#Merge Behavior|Directive: v-bind=“object” Merge Behavior]]
- [[vue3-directive-v-on#移除 .native 修饰符|移除 v-on:event.native 修饰符]]
- [[vue3-directive-v-for#ref 不再注册数组|v-for 的 ref 不再注册数组]]

### 组件

- [[vue3-functional-component|使用普通函数创建函数式组件]]
- [[vue3-global-api-defineAsyncComponent|异步组件使用 defineAsyncComponent 创建]]
- [[vue3-option-emits|组件事件需要在 emits 选项声明]]

### 渲染函数

- [[vue3-global-api-h|渲染函数 API 的变化]]
- `$scopedSlots` 被移除，所有插槽通过 `$slots` 访问 -> 没深究
  - [插槽统一 | Vue.js](https://v3.cn.vuejs.org/guide/migration/slots-unification.html#%E6%A6%82%E8%A7%88)
- `$listeners` 被移除，监听器对象被整合到 `$attrs` -> 没深究
  - https://v3.cn.vuejs.org/guide/migration/listeners-removed.html
- [[vue3-instance-property-\$attrs|$attrs 包含 class 和 style]]

### 自定义元素

- 自定义元素检测在编译时执行
- `is` 属性限制在 `component` 上 -> 没深究
  - https://v3.cn.vuejs.org/guide/migration/custom-elements-interop.html#%E8%87%AA%E4%B8%BB%E5%AE%9A%E5%88%B6%E5%85%83%E7%B4%A0

### 其他细节

- 生命周期选项名称变化：`destroyed` -> `unmounted`
- 生命周期选项名称变化：`beforeDestroy` -> `beforeUnmount`
- props 的 `default` 函数不再能够访问 `this` 上下文
  - 接受 `props` 作为参数，访问原始 props
  - 可以使用 [[vue3-global-api-inject]]
- [[vue3-concept-directive|自定义指令 API 与组件生命周期保持一致]]
- [[vue3-option-data|data 选项仅接受函数]]
- [[vue3-option-data|来自 mixin 的 data 选项现在仅会进行浅合并]]
- [[vue3-concept-attribute|移除枚举 Attribute 强制行为]]
- `<transition>` 的 class 名称变更
  - `v-enter` 修改为 `v-enter-from`
  - `v-leave` 修改为 `v-leave-from`
- [[vue3-component-transition-group|transition-group 元素不再默认渲染 wrapper 元素]]
- [[vue3-option-watch#侦听数组变化|在 watch 选项中使用 deep 选项侦听数组变化]]
- `<template>` 没有特殊指令时将被视作普通元素渲染
- [[vue3-concept-mount-point|挂载应用将不会替代元素]]
- 生命周期事件名称发生改变：前缀 `hook:` -> 前缀 `vnode-`
  - 例如：`@hook:updated="onUpdated"` -> `@vnode-updated="onUpdated"`

### 被移除的内容

- [[vue3-directive-v-on#移除按键修饰符 keyCode 支持|移除 v-on 按键修饰符]]
- 实例方法 `$on`、`$off`、`$once` 被移除 -> 没深究
  - [事件 API | Vue.js](https://v3.cn.vuejs.org/guide/migration/events-api.html#%E4%BA%8B%E4%BB%B6%E6%80%BB%E7%BA%BF)
- `filter` 被移除
  - 使用计算属性、 `config.globalProperties` 取代
- 内联模版被移除 -> 没深究
  - [内联模板 Attribute | Vue.js](https://v3.cn.vuejs.org/guide/migration/inline-template-attribute.html#%E6%A6%82%E8%A7%88)
- `$children` 实例属性（property）被移除
  - 建议通过 `$refs` 访问子组件实例
- `propsData` 被移除
  - 通过 `createApp` 第二个参数可以实现一样的效果
- `$destroy` 实例方法被移除
- 全局函数 `set` 和 `delete` 以及实例方法 `$set` 和 `$delete` 被删除
  - 基于 proxy 的变化监测已经不需要它们了

## 值得注意的新特性

- [[vue3-concept-composition-api|组合式 API]]
- [[vue3-component-teleport|内置组件 Teleport]]
- [[vue3-concept-fragments|片段：多根节点]]
- [[vue3-option-emits|组件选项：emits]]
- 自定义渲染器
  - [core/packages/runtime-core/src at main · vuejs/core · GitHub](https://github.com/vuejs/core/tree/main/packages/runtime-core/src)
- [[vue3-sfc-script-setup|单文件组件组合式 API 语法糖：script setup]]
- [[vue3-sfc-style#v-bind in Style|状态驱动的动态 CSS（v-bind in Style）]]
- [[vue3-component-suspense|Suspense （实验性）]]
