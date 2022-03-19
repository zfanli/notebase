---
type: Vue3
---

# Renderer: Simple Diff

渲染器需要对新旧元素进行比对，找到新旧 [[vue3-concept-virtual-dom|虚拟节点]] 发生的变化，用最小的代价完成对真实 DOM 元素的操作。简单 Diff 算法提供一种简易的思路完成新旧虚拟节点的对比，但并非最优选择。更进一步参考 [[vue3-renderer-diff-algo-double-end-diff|双端 Diff]]。

这里只关注 Diff 算法的内容，要完成渲染操作所需的函数列出如下，在下面的代码片段中将直接引用不再解释。

```js
/**
 * 将目标元素移动到指定容器中，指定锚点元素之前
 * @param {DOMElement} el 目标元素
 * @param {DOMElement} container 容器元素
 * @param {DOMElement} anchor 锚点元素
 */
function insert(el, container, anchor = null) {
  // 内容略
}

/**
 * 给新旧节点打补丁并移动到指定容器中，指定锚点元素之前
 * @param {VNode} n1 旧节点
 * @param {VNode} n2 新节点
 * @param {DOMElement} container 容器元素
 * @param {DOMElement} anchor 锚点元素
 */
function patch(n1, n2, container, anchor) {
  // 当旧节点 n1 不存在时执行挂载操作，元素挂在在 anchor 前
  // 档旧节点 n1 存在时执行打补丁操作
  // 其他内容略
}

/**
 * 卸载目标节点
 * @param {VNode} vnode 目标节点
 */
function unmount(vnode)) {
  // 内容略
}
```

## 简单 Diff 算法

WIP

```js
function patchChildren(n1, n2, container) {
  if (typeof n2.children === 'string') {
    // 内容略
  } else if (Array.isArray(n2.children)) {
  }
}
```

## 移动操作

测试数据：

新旧虚拟节点的差别在于 children 中元素的顺序。

```js
const oldVNode = {
  type: 'div',
  children: [
    { type: 'p', children: 'text 1', key: 1 },
    { type: 'p', children: 'text 2', key: 2 },
    { type: 'p', children: 'text 3', key: 3 },
  ],
}

const newVNode = {
  type: 'div',
  children: [
    { type: 'p', children: 'text 3', key: 3 },
    { type: 'p', children: 'text 1', key: 1 },
    { type: 'p', children: 'text 2', key: 2 },
  ],
}
```
