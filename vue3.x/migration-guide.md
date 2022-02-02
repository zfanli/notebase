---
type: Vue
tags: Vue3.x
---

# Vue 3.x 迁移指南

## 非兼容变更

### 全局 API

- [[app-api-createApp|createApp]]
- [[concept-tree-shaking|全局 API 支持 Tree-Shaking]]

### 模版指令

- [[directive-v-model|Directive: v-model]]
- [[attribute-key|Attribute: key]]
- [[directive-v-if-and-v-for|Directive: v-if & v-for]]
- [[directive-v-bind#Merge-Behavior|Directive: v-bind=“object” Merge Behavior]]
- [[directive-v-on#移除 .native 修饰符|移除 v-on:event.native 修饰符]]
- [[directive-v-for#ref 不再注册数组|v-for 的 ref 不再注册数组]]

### 组件

- [[functional-component|使用普通函数创建函数式组件]]
- [[app-api-defineAsyncComponent|异步组件使用 defineAsyncComponent 创建]]
- [[option-emits|组件事件需要在 emits 选项声明]]

### 渲染函数

- [[global-api-h|渲染函数 API 的变化]]
- `$scopedSlots` 被移除，所有插槽通过 `$slots` 访问
  - [插槽统一 | Vue.js](https://v3.cn.vuejs.org/guide/migration/slots-unification.html#%E6%A6%82%E8%A7%88)
- `$listeners` 被移除，监听器对象被整合到 `$attrs`
  - https://v3.cn.vuejs.org/guide/migration/listeners-removed.html
- [[instance-property-\$attrs|$attrs 包含 class 和 style]]

### 自定义元素

- 自定义元素检测在编译时执行
- `is` 属性限制在 `component` 上
  - https://v3.cn.vuejs.org/guide/migration/custom-elements-interop.html#%E8%87%AA%E4%B8%BB%E5%AE%9A%E5%88%B6%E5%85%83%E7%B4%A0

### 其他细节

- 生命周期选项名称变化：`destroyed` -> `unmounted`
- 生命周期选项名称变化：`beforeDestroy` -> `beforeUnmount`
- props 的 `default` 函数不再能够访问 `this` 上下文
  - 接受 `props` 作为参数，访问原始 props
  - 可以使用 [[global-api-inject]]
- [[concept-directive|自定义指令 API 与组件生命周期保持一致]]
- [[option-data|data 选项仅接受函数]]
