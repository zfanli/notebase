---
type: Snippet
tags: DataStructure Queue
---

# Snippet: Simple Queue

简单的队列实现。实现队列的入列、出列方法，同时实现队列查看首个元素、检查队列是否为空的方法。

## Java

```java
class SimpleQueue {
  private List<Integer> queue;
  private int head;

  public SimpleQueue() {
    queue = new ArrayList<>();
    head = 0;
  }

  public boolean enqueue(int x) {
    queue.add(x);
    return true;
  }

  public boolean dequeue() {
    if (isEmpty()) {
      return false
    }
    head++;
    return true;
  }

  public int peek() {
    return queue.get(head);
  }

  public boolean isEmpty() {
    return queue.size() <= head;
  }
}
```

## Python

```python
class SimpleQueue:
    queue = None
    head = None

    def __init__(self) -> None:
        self.queue = []
        self.head = 0

    def enqueue(self, x: int) -> bool:
        self.queue.append(x)
        return True

    def dequeue(self) -> bool:
        if self.is_empty():
            return False
        self.head += 1
        return True

    def peek(self) -> int:
        return self.queue[self.head]

    def is_empty(self) -> bool:
        return len(self.queue) <= self.head


if __name__ == "__main__":
    queue = SimpleQueue()
    print(queue.enqueue(0))
    print(queue.enqueue(1))
    print(queue.enqueue(2))
    print(queue.peek())
    print(queue.dequeue())
    print(queue.peek())
    print(queue.dequeue())
    print(queue.peek())
    print(queue.dequeue())
    print(queue.is_empty())
```

## JS

```js
class SimpleQueue {
  queue = null
  head = null

  constructor() {
    this.queue = []
    this.head = 0
  }

  enqueue(x) {
    this.queue.push(x)
    return true
  }

  dequeue() {
    if (this.isEmpty()) return false
    this.head++
    return true
  }

  peek() {
    return this.queue[this.head]
  }

  isEmpty() {
    return this.queue.length <= this.head
  }
}
```
