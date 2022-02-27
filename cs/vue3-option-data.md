---
type: Vue3
# tags: Vue3.x
created: 2022-02-02
---

# Option: data

非兼容。

- `data` 选项现在不再接受纯 JavaScript `object`，而是仅接受**函数**。
- 使用 mixin 或 extend 时多个 `data` 将进行浅合并，不再进行深合并。

## 迁移策略

重构避免依赖 mixin 的 `data` 合并。
