---
type: StateManagement
tags: StateManagement Vue JavaScript QoK-B
---

# Pinia

Pinia 是 Vue 状态管理的替代方案。同时适用于 Vue 2.x 和 Vue 3.x，在 2022 年 1 月的 VueConf 视频中，由 Vue 的作者 Evan You 提出 Pinia 作为目前 Vue 3.x 的推荐状态管理插件，取代以往的 Vuex 插件。

Pinia 的名字来自西语单词 piña，意思是菠萝，Pinia 的 logo 就是一个菠萝。

Pinia 特性如下：

- 开发工具支持
  - 提供时间线追踪 action 和 mutation
  - Store 只会出现在使用的组件中
  - 时间旅行和方便调试
- 模块热重载
  - 不更新页面的情况下修改 Store
  - 开发时保持所有状态
- 可通过插件扩展 Pinia
- 支持 TypeScript 并对 JS 用户提供自动补完支持
- 服务端渲染支持

## 与 Vuex 相比

Pinia 诞生于对下一代 Vuex 的探索，结合了核心团队对 Vuex 5 的很多想法。最终核心团队发现 Pinia 已经实现了他们对 Vuex 5 的大部分设想，于是决定用它作为新的推荐选项以取代 Vuex。

与 Vuex 相比，Pinia 提供了相似的 API 并简化了用法，提供组合式 API，最重要的是配合 TypeScript 使用时提供完整的类型接口。

### RFCs

Pina 目前成为默认状态管理方案，同其他 Vue 生态的库一样，Pinia 通过 RFC 流程来进行迭代，并且目前 API 进入稳定状态。

### 与 Vuex 3.x/4.x 对比

- 取消了 mutation 的显式定义。mutation 通常让人感觉啰嗦，现在他们不需要在代码中出现，开发工具中仍然能够监控 mutation。
- 原生支持 TypeScript，其 API 在设计时就考虑到尽可能支持 TypeScript。
- 无需关注动态添加 store 的限制，所有 store 默认都是动态添加的，对用户无感知。你可以手动添加 store，不再需要关注以往的限制。
- 不再有模块嵌套的结构。Pinia 提供一个扁平化的 store，需要时你可以在 store 中引用其他 store。甚至是循环引用的 store。
- 不再有命名空间模块。你可以用对象嵌套的方法来创造命名空间。Pinia 默认的扁平化结构也可以认为所有 store 都有对应的命名空间。

## 概念

- [[vue-pinia-store]]
- [[vue-pinia-state]]
- [[vue-pinia-getters]]
- [[vue-pinia-actions]]
- [[vue-pinia-plugins]]
