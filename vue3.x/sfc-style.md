---
type: Vue
tags: Vue3.x
created: 2022-02-09
---

# SFC: style

## style scoped

`<style scoped>` 让其中的 CSS 只应用到当前的组件上。

其通过 PostCSS 转换将以下内容：

```html
<style scoped>
  .example {
    color: red;
  }
</style>

<template>
  <div class="example">hi</div>
</template>
```

转换为下面这样来实现功能。

```html
<style>
  .example[data-v-f3f3eg9] {
    color: red;
  }
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```

### 子组件的根元素

父组件样式会影响到子组件根元素，实现调整布局的目的。

### 深度选择器

`<style scoped>` 中如果需要影响子组件，需要使用深度选择器 `:deep()`，其本质上是一个 pseudo class。

```html
<style scoped>
  .a :deep(.b) {
    /* ... */
  }
</style>
```

上面代码会被编译为：

```css
.a[data-v-f3f3eg9] .b {
  /* ... */
}
```

> `v-html` 创建的 DOM 不会被 `scoped` 样式影响，同样你可以使用深度选择器来设置其样式。

### 插槽选择器

默认 `scoped` 下设置的样式不会影响到插槽内到元素。使用 `:slotted()` pseudo class 可以将插槽内容作为选择器的目标。

```html
<style scoped>
  :slotted(div) {
    color: red;
  }
</style>
```

### 全局选择器

使用 `:global()` pseudo class 可以将制定样式应用到全局。

```html
<style scoped>
  :global(.red) {
    color: red;
  }
</style>
```

### 提示

- `scoped` 中标签选择器（如 `p { color: red }`）结合 attribute 选择器（如编译后的 `[data-v-f3f3eg9]`）使用时效率会慢很多倍。如果结合 class 或者 id 使用则可以避免这个性能损失（如 `.example { color: red }`）
- 小心递归组件中的后代选择器。比如 `.a .b` 来说，如果匹配到 `.a` 的元素递归引用自身，则子组件所有 `.b` 都会被应用这条样式规则

## v-bind in Style

新增。

状态驱动的动态 CSS。单文件组件的 `<style>` 标签可以通过 `v-bind` CSS 函数将 CSS 的值关联到动态的组件状态上。

实际的值会被编译成 hash 的 CSS 自定义 property，CSS 本身依然是静态的，自定义 property 会通过内联样式应用到组件的根元素上，并在源值发生变更时响应式更新。

```html
<template>
  <div class="text">hello</div>
</template>

<script>
  export default {
    data() {
      return {
        color: 'red',
      }
    },
  }
</script>

<style>
  .text {
    color: v-bind(color);
  }
</style>
```

这个语法同样适用于 [[sfc-script-setup|script setup]]，且支持字符串形式的 JavaScript 表达式。

```html
<script setup>
  const theme = {
    color: 'red',
  }
</script>

<template>
  <p>hello</p>
</template>

<style scoped>
  p {
    color: v-bind('theme.color');
  }
</style>
```
