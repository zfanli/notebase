---
type: Vue
tags: Vue JavaScript QoK-D
created: 2022-02-09
---

# Tips

不好归类的提示。

## QA

### 计算属性缓存 VS 方法

调用方法：

- 每次重新执行逻辑
- 对于不经常更新的数据源来说重复执行造成性能浪费

使用计算属性：

- 如果依赖未更新，则每次都返回上一回计算的缓存结果
- 如果依赖已更新，则重新执行计算过程
- 对于不经常更新数据源来说会使用缓存结果避免性能浪费
- 如果不希望有缓存，则使用方法
- 依赖 Vue 的响应式关系缓存，比如下面的代码使用计算属性时将永远不会更新

```js
computed: {
  now() {
    return Date.now()
  }
}
```

## Options: methods

### 防抖和节流

Vue 不提供内置的防抖和节流功能，需要使用第三方库如 Lodash。

需要注意，直接在 `method` 中应用防抖会造成多个实例共享相同的防抖函数。

```html
<script src="https://unpkg.com/lodash@4.17.20/lodash.min.js"></script>
<script>
  Vue.createApp({
    methods: {
      // 用 Lodash 的防抖函数
      click: _.debounce(function () {
        // ... 响应点击 ...
      }, 500),
    },
  }).mount('#app')
</script>
```

公用同一个防抖函数可能会在组件复用时产生问题，解决方法是在 `created` 生命周期钩子中设置防抖函数。

```js
app.component('save-button', {
  created() {
    // 使用 Lodash 实现防抖
    this.debouncedClick = _.debounce(this.click, 500)
  },
  unmounted() {
    // 移除组件时，取消定时器
    this.debouncedClick.cancel()
  },
  methods: {
    click() {
      // ... 响应点击 ...
    },
  },
  template: `
    <button @click="debouncedClick">
      Save
    </button>
  `,
})
```
