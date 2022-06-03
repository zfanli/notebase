---
type: DataStructure
tags: Queue BFS FIFO QoK-B
---

# Queue: First-in-first-out

队列，是一种 [[data-structure-fifo|先进先出]] 的数据结构，可以保证元素按照顺讯依次进行处理。

队列有两个基本操作，分别为：

- **入列（enqueue）**：插入操作（insert operation），将新的元素放置在队列的末尾；
- **出列（dequeue）**：删除操作（delete operation），移除队列的首个元素。

## 队列数据结构的实现

使用一个动态数组和一个 head 指针可以实现一个队列数据结构。参考 [[snippet-data-structure-simple-queue|简易队列实现]]。这种方法简单粗暴，但是无法适用所有场景。

最典型也是最常见的限制就是储存值的空间是有限的，而这个实现只能将新的元素添加在数组的右边。

这意味着当有元素出列，数组左边的空间将会被空出，但是这部分空间无法被利用起来，在数组右侧达到最大空间限制后，新的元素就无法再被添加入队列。

这时如果能在右侧空间用完后，也能按照顺序将左侧的空间也利用起来，就能最大限度的利用所有空间资源。这种数据结构被称作循环队列（Circular Queue），或者 [[data-structure-ring-buffer|环形缓冲区]]。参考 [[snippet-data-structure-circular-queue|循环队列]] 代码实现。这个数据结构适合缓存数据流，使用场景有网卡的数据交换、音视频流的传输等。

## 编程语言中的 Queue

队列作为一种基本的数据结构，在绝大多数流行的编程语言中都有内置的实现，所以一般情况下无需重复造轮子。
