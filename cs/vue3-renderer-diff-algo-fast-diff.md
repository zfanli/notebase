---
type: Vue
tags: Vue JavaScript QoK-D
---

# Renderer: Fast Diff

快速 Diff 算法。最初应用于 ivi 和 inferno 两个框架，Vue 3.x 借鉴并对其进行了扩展。快速 Diff 相较于 [[vue3-renderer-diff-algo-double-end-diff|双端 Diff]] 性能更加优秀。

快速 Diff 算法的核心在于：

1. 参考纯文本 Diff 的思路，对数据进行**预处理**来提升效率
2. 在需要移动 DOM 时计算**最长递增子序列**（LIS）来最小化 DOM 移动操作
