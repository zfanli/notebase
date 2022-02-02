---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Directive: v-model

非兼容。

Vue 2.x 用法。

```html
<ChildComponent v-model="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent :value="pageTitle" @input="pageTitle = $event" />
```

Vue 3.x 用法。

```html
<ChildComponent v-model="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent
  :modelValue="pageTitle"
  @update:modelValue="pageTitle = $event"
/>
```

## `v-model` 指定参数

Vue 3.x 支持 `v-model` 指定参数。

```html
<ChildComponent v-model:title="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
```

## `v-model` 支持多次绑定

也支持多个 `v-model` 属性。

```html
<ChildComponent v-model:title="pageTitle" v-model:content="pageContent" />

<!-- 是以下的简写： -->

<ChildComponent
  :title="pageTitle"
  @update:title="pageTitle = $event"
  :content="pageContent"
  @update:content="pageContent = $event"
/>
```

## Vue 2.x `v-model` 的限制

- `v-model` 绑定 key 为 `value` 的 prop，存在硬编码的关系
- `v-model` 绑定 `input` 事件更新绑定的值，存在硬编码的关系
- 一个组件只能使用一个 `v-model` 指令

## Vue 3.x 对 `v-model` 的改变

- `v-model` 默认绑定 key 变更为 `modelValue` 的 prop，且可以通过传参指定 key 名称，消除硬编码关系
- `v-model` 绑定的事件从 `input` 变更为 `update:modelValue`，消除对 `input` 事件的绑定
- `v-model` 在同一个组件可以使用多次以绑定多个值
- 移除 `v-bind` 的 `.sync` 修饰符的支持（`v-model` 传参替代）
- 移除 `model` 选项

### 移除 `model` 选项

Vue 2.x `model` 选项可以修改 `v-model` 默认绑定的 prop 名称和事件的名称，以解除对 `value` prop 和 `input` 事件的硬编码关系。

在 Vue 3.x 中通过传参和变更绑定事件名称已经消除了这些问题，`model` 选项也将被移除。

下面是 Vue 2.x 中使用 `model` 选项的示例。

```html
<!-- ParentComponent.vue -->

<ChildComponent v-model="pageTitle" />
```

```js
// ChildComponent.vue

export default {
  model: {
    prop: "title",
    event: "change",
  },
  props: {
    // 这将允许 `value` 属性用于其他用途
    value: String,
    // 使用 `title` 代替 `value` 作为 model 的 prop
    title: {
      type: String,
      default: "Default title",
    },
  },
}
```

上面的定义是对下面写法的简写。

```html
<ChildComponent :title="pageTitle" @change="pageTitle = $event" />
```

### 迁移策略

- 将代码库使用 `.sync` 的地方替换为 `v-model`
- 使用不带参数的 `v-model` 的地方确保绑定的参数和事件正确设置为 `modelValue` 和 `update:modelValue`

> Vue 3.x 新增了 [[option-emits|emits 选项]] 定义组件真正可能触发的事件。`update:modelValue` 也需要定义到其中。参考下面代码。

```html
<ChildComponent v-model="pageTitle" />
```

```js
// ChildComponent.vue

export default {
  props: {
    modelValue: String, // 以前是`value：String`
  },
  emits: ["update:modelValue"],
  methods: {
    changePageTitle(title) {
      this.$emit("update:modelValue", title) // 以前是 `this.$emit('input', title)`
    },
  },
}
```
