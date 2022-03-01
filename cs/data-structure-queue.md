---
type: DataStructure
tags: Queue BFS
---

# Queue: First-in-first-out

队列，是一种 [[data-structure-fifo|先进先出]] 的数据结构。

队列有两个基本操作，分别为：

- **入列（enqueue）**：插入操作（insert operation），将新的元素放置在队列的末尾；
- **出列（dequeue）**：删除操作（delete operation），移除队列的首个元素。

## 实现队列数据结构

使用一个动态数组和一个 head 指针可以实现一个队列数据结构。参考 [[snippet-data-structure-simple-queue]]。
