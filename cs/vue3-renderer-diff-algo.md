---
type: Vue
---

# Renderer: Diff Algorithm

Diff 算法是渲染器的核心。渲染器用其来检出 [[vue3-concept-virtual-dom|虚拟节点]] 中发生了哪些变化，并将这些变化转换为最小消耗的 [[concept-imperative|命令式]] 语句，来完成最终的 DOM 更新操作。

Vue 3.x 中使用了 [[vue3-renderer-diff-algo-fast-diff|快速 Diff]] 算法来完成这一目的。

[[vue-design|Vuejs 设计与实现]] 一书中从浅到深介绍了如何实现一个 Diff 算法来实现最小消耗检出虚拟节点的变化。书中分别列举了思路比较直接的 [[vue3-renderer-diff-algo-simple-diff|简单 Diff]] 算法，以及真实 DOM 操作更少的 [[vue3-renderer-diff-algo-double-end-diff|双端 Diff]] 算法，来阐述实现一个 Diff 算法的过程中会遇到的难点与解决的方案。书中最后也介绍了 Vue 3.x 中使用的 [[vue3-renderer-diff-algo-fast-diff|快速 Diff]] 算法的思路和实现方法，通过对数据结构和算法的处理来达到更好的性能。
