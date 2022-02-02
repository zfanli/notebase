---
type: Vue
tags: Vue3.x
created: 2022-02-01
---

# Directive: v-on

## 移除 .native 修饰符

非兼容。

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

## 移除按键修饰符 keyCode 支持

非兼容。

- 不再支持数字键码作为 `v-on` 的修饰符
- 不在支持 `config.keyCodes`

### Vue 2.x 用法

`keyCode` 可以作为 `v-on` 的修饰符。

```html
<!-- 键码版本 -->
<input v-on:keyup.13="submit" />

<!-- 别名版本 -->
<input v-on:keyup.enter="submit" />
```

`config.keyCodes` 可以用来定义按键的别名。

```js
Vue.config.keyCodes = {
  f1: 112,
}
```

```html
<!-- 键码版本 -->
<input v-on:keyup.112="showHelpText" />

<!-- 自定义别名版本 -->
<input v-on:keyup.f1="showHelpText" />
```

### Vue 3.x 用法

`keyCode` 将被废弃，所以相关支持已经没有意义。Vue 3.x 移除了对其的支持，现在可以通过键的名称作为修饰符，复合词使用 kebab-cased（横线）命名。

> 参考。
>
> [KeyboardEvent.keyCode - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/keyCode)

```html
<!-- Vue 3 在 v-on 上使用按键修饰符 -->
<input v-on:keyup.page-down="nextPage" />

<!-- 同时匹配 q 和 Q -->
<input v-on:keypress.q="quit" />
```
