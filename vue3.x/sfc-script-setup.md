---
type: Vue
tags: Vue3.x
created: 2022-02-09
---

# SFC: script setup

新增。

`<script setup>` 是单文件组件（SFC）中使用组合式 API 的编译时语法糖。相比普通 `<script>` 语法，其优势如下：

- 代码更加简洁
- 可以使用纯 TypeScript 声明 props 和 emits
- 更好的运行时性能（会被编译成渲染函数）
- 更好的 IDE 类型推断性能（减少语言服务器从代码中抽离类型的工作）

## 用法

`<script setup>` 的内容会被编译为组件的 `setup` 选项。**会在每次组件被实例化时执行**。

```html
<script setup>
  console.log('hello script setup')
</script>
```

### 顶层绑定直接暴露给模版

在 `<script setup>` 中顶层的绑定的变量、函数声明会直接暴露给模版。

```html
<script setup>
  // 变量
  const msg = 'Hello!'

  // 函数
  function log() {
    console.log(msg)
  }
</script>

<template>
  <div @click="log">{{ msg }}</div>
</template>
```

通过 `import` 导入的内容也可以直接暴露给模版，而无需经过 `methods` 选项。

```html
<script setup>
  import { capitalize } from './helpers'
</script>

<template>
  <div>{{ capitalize('hello') }}</div>
</template>
```

### 响应式

响应式 API 在模版中可以被自动解包。

```html
<script setup>
  import { ref } from 'vue'

  const count = ref(0)
</script>

<template>
  <button @click="count++">{{ count }}</button>
</template>
```

### 使用组件

`import` 进来的组件可以直接在模版中通过变量名称使用。

```html
<script setup>
  import MyComponent from './MyComponent.vue'
</script>

<template>
  <MyComponent />
</template>
```

由于是变量名称绑定，需要动态组件时需要使用 `:is` 属性来绑定。注意在三元表达式中，组件是通过变量引用而非字符串。

```html
<script setup>
  import Foo from './Foo.vue'
  import Bar from './Bar.vue'
</script>

<template>
  <component :is="Foo" />
  <component :is="someCondition ? Foo : Bar" />
</template>
```

### 递归组件

单文件组件中可以通过其文件名称对自己进行引用。比如 `FooBar.vue` 组件在模版中可以通过 `<FooBar />` 对自己进行引用。

需要注意这种引用的优先级较低，如果 `<script setup>` 存在一个 `FooBar` 的变量则会被覆盖。如果名称存在冲突，可以使用别名来对 `import` 进来的属性设置别名。

### 命名空间组件

可以通过命名空间引用组件。

```html
<script setup>
  import * as Form from './form-components'
</script>

<template>
  <Form.Input>
    <Form.Label>label</Form.Label>
  </Form.Input>
</template>
```

### 自定义指令

`<script setup>` 中定义的局部指令必须以 `vNameOfDirective` 命名才能正常使用。

```html
<script setup>
  const vMyDirective = {
    beforeMount: (el) => {
      // 在元素上做些操作
    },
  }
</script>
<template>
  <h1 v-my-directive>This is a Heading</h1>
</template>
```

```html
<script setup>
  // 导入的指令同样能够工作，并且能够通过重命名来使其符合命名规范
  import { myDirective as vMyDirective } from './MyDirective.js'
</script>
```

### defineProps 和 defineEmits

`<script setup>` 提供 `defineProps` 和 `defineEmits` **编译器宏 API** 来声明 `props` 和 `emits` 选项。它们不需要导入，可以直接使用。

限制是这两个 API 不能使用 `<script setup>` 中声明的局部变量来设置其内容，否则将引起编译报错。

```html
<script setup>
  const props = defineProps({
    foo: String,
  })

  const emit = defineEmits(['change', 'delete'])
  // setup code
</script>
```

### defineExpose

`<script setup>` 暴露的变量和函数通过模版 `ref` 或者 `$parent` 变量是无法访问到的。你需要使用 `defineExpose` 编译器宏 API 来显式声明哪些内容是可以被外部访问的。

```html
<script setup>
  import { ref } from 'vue'

  const a = 1
  const b = ref(2)

  defineExpose({
    a,
    b,
  })
</script>
```

### useSlots 和 useAttrs

在 `<script setup>` 访问 `$slots` 和 `$attrs` 属性。

```html
<script setup>
  import { useSlots, useAttrs } from 'vue'

  const slots = useSlots()
  const attrs = useAttrs()
</script>
```

## TypeScript 支持

### defineProps 和 defineEmits 类型声明

如果使用 TypeScript，可以使用类型声明来定义 `props` 和 `emits` 选项。

```ts
const props = defineProps<{
  foo: string
  bar?: number
}>()

const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
```

类型声明和运行时声明需要二选一，同时使用会造成编译报错。

目前类型声明限制如下：

- 开发环境中，编译器会尝试对变量进行类型推断，比如字符串值的变量会被推断为 `String` 类型，但是外部导入的变量因为编译器没有外部文件的信息，则会被推断为 `null`
- 生产模式，类型声明会被生成为数组以减少打包体积
- 静态分析会生成带类型信息的 TypeScript 代码用来在后续的流程中处理
- 暂时不支持外部引用的类型

### 类型声明时的默认 props 值

类型推断不足之处在于没有给 props 提供默认值的方式。`withDefaults` 编译器宏 API 可以解决这个问题。

```ts
interface Props {
  msg?: string
  labels?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  msg: 'hello',
  labels: () => ['one', 'two'],
})
```

## 限制

不能使用 `src` 属性将 `<script setup>` 的内容放到其他文件中。
