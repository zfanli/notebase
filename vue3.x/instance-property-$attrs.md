---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Instance Property: $attrs

非兼容。

`$attrs` 现在包括所有传给组件的属性，包括 `class` 和 `style`。

## Vue 2.x 行为

Vue 2.x 的虚拟 DOM 实现对 `class` 和 `style` 存在特殊处理，所以这俩属性不被包含在 `$attrs` 中。这会导致使用 `inheritAttrs: false` 选项时产生副作用：

- 使用了 `inheritAttrs: false` 选项后 `$attrs` 定义的属性将不会自动添加在根节点上
- `class` 和 `style` 不在 `$attrs` 范围内，这导致它们依旧被添加到根节点上

定义：

```html
<template>
  <label>
    <input type="text" v-bind="$attrs" />
  </label>
</template>
<script>
  export default {
    inheritAttrs: false,
  }
</script>
```

使用：

```html
<my-component id="my-id" class="my-class"></my-component>
```

渲染结果：

```html
<label class="my-class">
  <input type="text" id="my-id" />
</label>
```

## Vue 3.x 行为

Vue 3.x 将 `class` 和 `style` 也包括在 `$attrs` 内。所以上面的例子会渲染出下面的结果。

```html
<label>
  <input type="text" id="my-id" class="my-class" />
</label>
```

## 迁移策略

使用了 `inheritAttrs: false` 时，`class` 和 `style` 添加位置变化会导致视觉样式出错，需要检查相关位置的视觉样式是否符合预期。
