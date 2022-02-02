---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Directive: v-on

## 移除 `.native` 修饰符

Vue 2.x 对于 Vue 组件来说，`v-on` 只会监听由 `this.$emit` 触发的事件，如果希望在 Vue 组件上监听 DOM 原生事件，需要加上 `.native` 修饰符。

```html
<my-component
  v-on:close="handleComponentEvent"
  v-on:click.native="handleNativeClickEvent"
/>
```

在 Vue 3.x 中移除了 `v-on` 指令的 `.native` 修饰符。同时新增了 [[option-emits|emits]] 选项允许自组件定义真正会被触发的事件。

组件未定义的事件将进行监听 DOM 原生事件。

```html
<my-component
  v-on:close="handleComponentEvent"
  v-on:click="handleNativeClickEvent"
/>
```

```html
<script>
  export default {
    emits: ["close"],
  }
</script>
```
