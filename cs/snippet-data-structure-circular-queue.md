---
type: Snippet
tags: DataStructure Queue FIFO QoK-D
---

# Snippet: Circular Queue

循环队列实现，设计一个循环队列，初始化为一个固定尺寸的队列，需要实现以下方法：

- `MyCircularQueue(k)` Initializes the object with the size of the queue to be k.
- `int Front()` Gets the front item from the queue. If the queue is empty, return -1.
- `int Rear()` Gets the last item from the queue. If the queue is empty, return -1.
- `boolean enQueue(int value)` Inserts an element into the circular queue. Return true if the operation is successful.
- `boolean deQueue()` Deletes an element from the circular queue. Return true if the operation is successful.
- `boolean isEmpty()` Checks whether the circular queue is empty or not.
- `boolean isFull()` Checks whether the circular queue is full or not.

参考 [[data-structure-queue|队列]] 和 [[data-structure-ring-buffer|环形缓冲区]]。

## Python

```python
class MyCircularQueue:

    def __init__(self, k: int):
        self.queue = [None] * k
        self.front = self.rear = -1
        self.maxlen = k


    def enQueue(self, value: int) -> bool:
        if self.isFull():
            return False

        # rear 为 -1 时队列为空，进行一轮初始化赋值
        if self.rear == -1:
            self.front = self.rear = 0

        self.queue[self.rear] = value
        self.rear = (self.rear + 1) % self.maxlen

        return True


    def deQueue(self) -> bool:
        if self.isEmpty():
            return False

        self.front = (self.front + 1) % self.maxlen

        # 两个指针重叠时，队列为空，重置指针位置到 -1
        if self.front == self.rear:
            self.front = self.rear = -1
        return True


    def Front(self) -> int:
        if self.isEmpty():
            return -1

        return self.queue[self.front]


    def Rear(self) -> int:
        if self.isEmpty():
            return -1

        return self.queue[self.rear - 1]


    def isEmpty(self) -> bool:
        # 指针指向 -1 表示队列为空
        return self.front == -1


    def isFull(self) -> bool:
        return not self.isEmpty() and self.front == self.rear

```

## 参考

- [Design Circular Queue - LeetCode](https://leetcode.com/explore/learn/card/queue-stack/228/first-in-first-out-data-structure/1337/)
