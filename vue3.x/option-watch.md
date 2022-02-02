---
type: Vue
tags: Vue3.x
created: 2022-02-02
---

# Option: watch

## 侦听数组变化

非兼容。

默认情况 `watch` 选项侦听数组变化时，仅数组被替换才会触发回调。使用 `deep` 选项可以侦听数组的变化。

```js
watch: {
  bookList: {
    handler(val, oldVal) {
      console.log('book list changed')
    },
    deep: true
  },
}
```

### 迁移策略

如果依赖数组变化的侦听，需要确保设置了 `deep` 选项。
