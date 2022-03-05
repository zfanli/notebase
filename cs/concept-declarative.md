---
type: Concept
tags: Concept
---

# Concept: Declarative 声明式

声明式，在概念上只关注结果，而不定义达到这个结果的过程是怎样的。与之相对的是 [[concept-imperative|命令式]]。

这个概念是广泛的、通用的，在各个领域都存在对应的命令式和声明式的实现。

## 前端领域的声明式实现

前端领域的流行框架，比如 Vue.js 和 React 都属于声明式开发框架。其优势在于仅需关注期望的结果，对开发和维护造成更小的心智负担。缺点在于声明式的底层依旧依赖命令式代码完成具体操作，其性能消耗等于“找到差异” + “执行命令式代码”，无法超越纯命令式代码。框架上能做的只有最小化找出差异这个过程。

比如 Vue.js 使用 [[vue3-concept-virtual-dom|虚拟 DOM]] 来最小化找到声明式定义的模版差异的消耗，在框架内部封装了命令式的代码来执行最终的 DOM 更新。