---
type: Vue
tags: Vue3.x
created: 2022-02-02
---

# Concept: Composition API

组合式 API 解决下面的问题：

- 组件规模膨胀之后，不同逻辑关注点分散在各个实例选项
- 这导致增加了理解和维护的成本
- 如果相同逻辑关注点放在一起会更好维护

组合式 API 将以往组件实例选项 API 才能实现的事情抽离出来，让一段需要初始化、侦听和销毁的逻辑从原本分散在选项式 API 各个生命周期方法中抽离，整合到一个位置。

抽离出来的逻辑还可以抽象成方法，只需要返回所需的响应式属性和指定方法，让一段逻辑变成可插拔式。提升了灵活性和可复用性。

## `setup` 组件选项

按照设计，[[[[option-setup]]|setup]] 中的代码在 `props` 解析之后，`data` 、 `computed` 和 `method` 等选项解析之前执行，所以无法使用 `this` 上下文。`setup` 中无法获取到组件实例。

`setup` 选项返回一个对象，可以包含响应式属性和方法，这些方法在其他实例选项中可以被访问。

组合式 API 关键依赖以下 Vue 3.x 新增的特性。

### 全局 API `ref`

新的 [[global-api-ref|全局 API ref]] 生成响应式变量，返回的变量是 `{ value: 'actualValue' }` 结构，`value` 字段是具体的值。这个数据结构的原因在于要统一引用类型和值类型变量的处理。

```js
import { ref } from "vue"

const counter = ref(0)

console.log(counter) // { value: 0 }
console.log(counter.value) // 0

counter.value++
console.log(counter.value) // 1
```

### 在 `setup` 内注册生命周期钩子

`setup` 选项内可以完成所有需要在各个实例选项中实现的操作。Vue 3.x 提供了一套全局 API 用来在组合式 API 中针对组件的各个生命周期进行逻辑处理。

> `beforeCreate` 和 `created` 不需要生命周期方法，在 `setup` 选项中时逻辑都将在这个时机执行。

- [[global-api-onBeforeMount|全局 API onBeforeMount]]
- [[global-api-onMounted|全局 API onMounted]]
- [[global-api-onBeforeUpdate|全局 API onBeforeUpdate]]
- [[global-api-onUpdated|全局 API onUpdated]]
- [[global-api-onBeforeUnmount|全局 API onBeforeUnmount]]
- [[global-api-onUnmounted|全局 API onUnmounted]]
- [[global-api-onErrorCaptured|全局 API onErrorCaptured]]
- [[global-api-onRenderTracked|全局 API onRenderTracked]]
- [[global-api-onRenderTriggered|全局 API onRenderTriggered]]
- [[global-api-onActivated|全局 API onActivated]]
- [[global-api-onDeactivated|全局 API onDeactivated]]

### 全局 API `computed`

[[global-api-computed|全局 API computed]] 用来在 Vue 外部创建计算属性。

```js
import { ref, computed } from "vue"

const counter = ref(0)
const twiceTheCounter = computed(() => counter.value * 2)

counter.value++
console.log(counter.value) // 1
console.log(twiceTheCounter.value) // 2
```

### 全局 API `watch`

[[global-api-watch|全局 API watch]] 用来做与实例选项 `watch` 相同的事情。它第一个参数接受侦听的对象，第二个参数是回调函数。

```js
import { ref, watch } from "vue"

const counter = ref(0)
watch(counter, (newValue, oldValue) => {
  console.log("The new counter value is: " + counter.value)
})
```

等价于下面选项 API：

```js
export default {
  data() {
    return {
      counter: 0,
    }
  },
  watch: {
    counter(newValue, oldValue) {
      console.log("The new counter value is: " + this.counter)
    },
  },
}
```

## 参考

- [什么是组合式 API？ | Vue.js](https://v3.cn.vuejs.org/guide/composition-api-introduction.html#%E4%BB%80%E4%B9%88%E6%98%AF%E7%BB%84%E5%90%88%E5%BC%8F-api)
