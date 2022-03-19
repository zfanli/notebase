---
type: vue3
---

# Renderer: Double-end Diff

双端 Diff，在 [[vue3-renderer-diff-algo-simple-diff|简单 Diff]] 的基础上进行了优化，使用新旧元素数组首尾索引共四个指针，从数组的两端开始同步进行比对，可以有效减少 DOM 操作次数。
