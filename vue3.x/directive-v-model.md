---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Directive: v-model

非兼容。

## 用法

### 表单元素双向绑定

`v-model` 指令在表单元素 `<input>`、`<textarea>` 和 `<select>` 上使用时会根据元素类型自动选取正确的方法来更新元素。

- text 和 textarea 元素使用 `value` property 和 `input` 事件
- checkbox 和 radio 使用 `checked` property 和 `change` 事件
- select 元素使用 `value` 作为 property 和 `change` 事件

> 需要输入法输入的语言，在文字组织的过程中不会进行数据的更新。如果你需要响应这些更新，需要使用 `input` 事件监听器和 `value` 绑定取代 `v-model`。

在 checkbox 上使用 `v-model` 时，根据选中状态会默认使用布尔值作为其绑定的具体值，可以通过下面的方式修改。

```html
<input type="checkbox" v-model="toggle" true-value="yes" false-value="no" />
```

```js
// 当选中时：
vm.toggle === 'yes'
// 当未选中时：
vm.toggle === 'no'
```

### 修饰符

#### .lazy

`.lazy` 会让 `v-model` 从默认监听 `input` 事件改为监听 `change` 事件。

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" />
```

#### .number

`.number` 在 `type="text"` 时可以自动将值转换为数值。如果这个值无法被 `parseFloat()` 解析，将返回原始的值。

```html
<input v-model.number="age" type="text" />
```

在 `type="number"` 时 Vue 会自动将值转换为数值，不需要指定 `.number` 修饰符。

#### .trim

`.trim` 可以自动过滤用户输入值的首位空白字符。

```html
<input v-model.trim="msg" />
```

## Vue 2.x 对比 Vue 3.x

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
    prop: 'title',
    event: 'change',
  },
  props: {
    // 这将允许 `value` 属性用于其他用途
    value: String,
    // 使用 `title` 代替 `value` 作为 model 的 prop
    title: {
      type: String,
      default: 'Default title',
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
  emits: ['update:modelValue'],
  methods: {
    changePageTitle(title) {
      this.$emit('update:modelValue', title) // 以前是 `this.$emit('input', title)`
    },
  },
}
```
