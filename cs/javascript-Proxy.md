---
type: JavaScript
tags: ES6 JavaScript ExoticObject
---

# Proxy

Proxy 是一个特殊 [[javascript-exotic-object|异质对象]]，其提供了与 [[javascript-ordinary-object|常规对象]] 完全一致的内部方法，并且可以通过代理配置来自定义这些内部方法的行为。

其本质上只能对代理对象本身的内部方法进行定制化，但是通过这个特性，我们可以拦截针对代理对象的操作，来对目标对象执行相应的操作，以此达到拦截目标对象内部方法的效果。

代理对象在创建时如果未对指定内部方法设置拦截函数，相关操作会直接传递给原始对象来完成。这是代理透明性质。

```js
const proxy = new Proxy(target, handler)
```

- `target` 需要代理的对象，可以是任何对象，包括函数
- `handler` 代理配置：一个对象，里面配置了 traps（夹子），来拦截对象的操作，比如 `get` 夹子拦截属性的读操作，`set` 夹子拦截属性的写操作

代理配置中如果存在相应的夹子拦截指定操作，那么在相应操作发生的时候，指定的夹子会被执行来处理这个操作。如果没有定义相应的夹子，操作会直接传递给原始对象。

下表是 Proxy 可以代理的对象内部方法，以及方法调用的时机。

| Internal Method         | Handler Method             | Triggers when...                                                                              |
| ----------------------- | -------------------------- | --------------------------------------------------------------------------------------------- |
|                         |                            |
| `[[Get]]`               | `get`                      | reading a property                                                                            |
| `[[Set]]`               | `set`                      | writing to a property                                                                         |
| `[[HasProperty]]`       | `has`                      | in operator                                                                                   |
| `[[Delete]]`            | `deleteProperty`           | delete operator                                                                               |
| `[[Call]]`              | `apply`                    | function call                                                                                 |
| `[[Construct]]`         | `construct`                | new operator                                                                                  |
| `[[GetPrototypeOf]]`    | `getPrototypeOf`           | Object.getPrototypeOf                                                                         |
| `[[SetPrototypeOf]]`    | `setPrototypeOf`           | Object.setPrototypeOf                                                                         |
| `[[IsExtensible]]`      | `isExtensible`             | Object.isExtensible                                                                           |
| `[[PreventExtensions]]` | `preventExtensions`        | Object.preventExtensions                                                                      |
| `[[DefineOwnProperty]]` | `defineProperty`           | Object.defineProperty, Object.defineProperties                                                |
| `[[GetOwnProperty]]`    | `getOwnPropertyDescriptor` | Object.getOwnPropertyDescriptor, for..in, Object.keys/values/entries                          |
| `[[OwnPropertyKeys]]`   | `ownKeys`                  | Object.getOwnPropertyNames, Object.getOwnPropertySymbols, for..in, Object.keys/values/entries |
