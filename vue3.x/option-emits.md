---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Option: emits

新增。

`emits` 选项声明组件中会触发的事件名称。这个选项接受数组和对象类型数据。

- 数组：定义事件名称
- 对象：定义事件名称、定义事件参数校验器

定义事件名称的例子。如果定义了原生事件，如 `click` 事件，将使用组件中的事件代替原生事件监听器。

```js
app.component("custom-form", {
  emits: ["inFocus", "submit"],
})
```

定义对象进行事件参数校验的例子。返回布尔值指示事件是否有效。

```js
app.component("custom-form", {
  emits: {
    // 没有验证
    click: null,

    // 验证 submit 事件
    submit: ({ email, password }) => {
      if (email && password) {
        return true
      } else {
        console.warn("Invalid submit event payload!")
        return false
      }
    },
  },
  methods: {
    submitForm(email, password) {
      this.$emit("submit", { email, password })
    },
  },
})
```

## 迁移策略

官方强烈建议给所有组件定义 `emits` 属性声明其可能触发的事件。任何未声明在 `emits` 中的事件的监听器会被挂在在组件的 `$attr` 属性上，并默认绑定到组件的根节点上。

### 未声明 `emits` 的情况

比如以下组件中，触发了 `click` 事件但是未在 `emits` 进行声明。

```html
<template>
  <button v-on:click="$emit('click', $event)">OK</button>
</template>
<script>
  export default {
    emits: [], // 不声明事件
  }
</script>
```

其他组件监听了 `click` 事件。

```html
<my-button v-on:click="handleClick"></my-button>
```

这时 `click` 事件被触发 2 次：

- 1 次来自 DOM 原生元素 `button` 的点击事件
- 另一次来自 `$emit('click', $event)`

你可以：

- 定义 `emits` 仅让 `$emit()` 来触发事件，这对 `click` 有额外处理逻辑时非常有用
- 删除 `$emit()`，仅让原生元素的监听起起效
