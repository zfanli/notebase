---
type: JavaScript
tags: JavaScript QoK-B
---

# Object.defineProperty()

```js
Object.defineProperty(obj, prop, descriptor)
```

一个静态方法，可以：

- 给对象添加新的属性
- 修改对象的现有属性

然后返回这个对象本身。

正常方法赋值添加的属性可以通过 `for ... in` 和 [[javascript-Object.keys]] 等方法枚举，并且属性的值可以被修改、属性本身可以被删除。`Object.defineProperty` 提供了修改这些默认行为等方法，其添加的属性默认是 **不可修改且不可枚举** 的。

`descriptor` 中可以配置如下属性：

- `configurable` 描述属性的描述符（descriptor）是否可以被修改，属性本身是否能被删除，默认是 false，即不可修改
- `enumerable` 描述属性是否可以被枚举，默认是 false，即不可枚举
- `value` 描述属性的值，默认是 `undefined`
- `writable` 描述属性的值是否能被修改，默认为 false，即不可修改
- `set` 访问器描述符，属性的 setter 函数，默认是 `undefined`
- `get` 访问器描述符，属性的 getter 函数，默认是 `undefined`

数据描述符（data descriptor）`value` 和 `writable` 同访问器描述符（accessor descriptor）`set` 和 `get` 这两组属性是互斥的，如果同时定义它们则会抛出错误。
