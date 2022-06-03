---
type: Vue
tags: Vue JavaScript QoK-B
created: 2022-02-09
---

# Concept: Fragments

新增。

片段指的是多根节点的组件。

在 Vue 2.x 中仅支持单根节点组件，即 `<template>` 下只能包含一个根节点，否则会编译工具会进行警告。

在 Vue 3.x 中支持多个根节点，注意在使用多个根节点时 Vue 无法知道该将外部传入的 `$attrs` 放在哪个根节点上，所以需要手动显式指定。

以下 2.x 的写法可以去除额外的 `div` 根节点。

```html
<!-- Layout.vue -->
<template>
  <div>
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
  </div>
</template>
```

以下是 3.x 的写法，注意 `$attrs` 的处理。

```html
<!-- Layout.vue -->
<template>
  <header>...</header>
  <main v-bind="$attrs">...</main>
  <footer>...</footer>
</template>
```
