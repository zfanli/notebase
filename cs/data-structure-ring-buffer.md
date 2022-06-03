---
type: DataStructure
tags: Queue FIFO QoK-B
---

# Ring Buffer

环形缓冲区，一种固定尺寸、头尾相连的 [[data-structure-queue|Queue]] 数据结构，适合缓存数据流。

也叫做循环队列（Circular Queue）、循环缓冲区（Cyclic Buffer、Circular Buffer）。

参考代码实现 [[snippet-data-structure-circular-queue|循环队列]]。

## 概念

环形缓冲区是一种经过设计的数据结构，在有限的空间中实现一个队列数据结构，元素从左到右依次入列，一直到队列被填满，新的元素无法入列。

而如果发生元素出列，队列左边会留出空位，此时新的元素可以按照顺序继续从左向右占满空位，直到队列被填满为止。

此时队列逻辑上形成一个环，任何位置都可能腾出空间，让新的元素可以加入。但实际上数据依然储存在连续的线性空间中，依靠指针操作来维护这个逻辑上的环形结构。

## 使用场景

网卡的数据中转；视频、音频流的传输等场景都能使用到环形缓冲区。
