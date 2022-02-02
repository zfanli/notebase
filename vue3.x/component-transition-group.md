---
type: Vue
tags: Vue3.x
created: 2022-02-02
---

# Transition Group 根元素

非兼容。

## Vue 2.x 用法

2.x 中 `<transition-group>` 需要根元素，默认会使用 `<span>` 作为外层根元素，可以使用 `tag` 属性修改标签。

```html
<transition-group tag="ul">
  <li v-for="item in items" :key="item">{{ item }}</li>
</transition-group>
```

## Vue 3.x 用法

3.x 中 `<transition-group>` 不再需要根元素，但是保留了 `tag` 属性的支持。如果代码依赖 `<span>` 元素包裹，可以使用下面的写法让其与 2.x 的默认行为一致。

```html
<transition-group tag="span">
  <!-- -->
</transition-group>
```
