---
type: Vue
tags: Vue3.x
created: 2022-02-02
---

# Concept: Attribute

## 枚举 Attribute 强制行为已被移除

非兼容。

> 这是一个底层内部 API 改动，对绝大部分开发者不会产生影响。

- 移除枚举 Attribute 内部概念，将这些属性视为普通非布尔值属性
- 如果属性值为 `false` 将不会移除该属性，而是将属性设值为 `attr="false"`
- 如果属性值为 `null`、`undefined` 将会移除该属性

### 参考

- [attribute 强制行为 | Vue.js](https://v3.cn.vuejs.org/guide/migration/attribute-coercion.html#%E5%B0%86-false-%E5%BC%BA%E5%88%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA-false-%E8%80%8C%E4%B8%8D%E6%98%AF%E7%A7%BB%E9%99%A4-attribute)
