---
type: Vue
tags: Vue JavaScript QoK-B
---

# Concept: Virtual DOM

虚拟 DOM 在 Vue.js 框架中作为实现 [[concept-declarative|声明式]] 的基础，用来找出用户定义的声明式代码中的差异部分，在需要的时候使用 [[concept-imperative|命令式]] 代码对真实的 DOM 进行更新。

Vue.js 框架所实现的声明式开发模式的执行性能无法超越命令式的原生 JavaScript 指令，但是相对于需要用户实现完整步骤的命令式开发模式，Vue.js 提供的声明式开发模式更加简化开发过程和利于维护。

在 [[vue-design|Vue.js 的设计与实现]] 一书中对虚拟 DOM 的性能做了一定对比。虚拟 DOM 的性能介于原生命令式 API 和 `innerHTML` 属性之间，使用前者表示完完全全的命令式开发模式，开发者的心智会有更重的负担；而后者接近声明式的开发模式，但是 `innerHTML` 中的 HTML 模版发生任何修改都需要重复销毁旧 DOM、新建所有 DOM 的过程，性能糟糕。虚拟 DOM 则会用比较算法找出每次需要更新的差异，然后使用命令式代码进行实际的更新。让开发者可以使用声明式的逻辑进行开发，产生更少的心智负担，并且在性能上也不会有太大损耗。
