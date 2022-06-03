---
type: JavaScript
tags: JavaScript QoK-B
---

# Ordinary Object 常规对象

常规对象指满足下面三点要求的对象：

1. 对于内部方法 `[[Call]]`，必须使用 ECMA 规范 10.2.1 节给出的定义实现
2. 对于内部方法 `[[Constructor]]`，必须使用 ECMA 规范 10.2.2 节给出的定义实现
3. 对于其他内部方法，必须使用 ECMA 规范 10.1.x 节给出的定义实现

不满足这些条件的对象为 [[javascript-exotic-object|异质对象]]
