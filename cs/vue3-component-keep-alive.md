---
type: Vue
---

# Component: keep-alive

`<keep-alive>` 内置组件受到渲染器的底层支持，可以让其 children 在卸载阶段不会被真正卸载，而是将 [[vue3-concept-virtual-dom|虚拟节点]] 缓存起来，并将真实 DOM 移动到一个隐藏的容器中保存。这样在组件重新激活时可以将缓存中的虚拟节点和真实 DOM 直接搬运回需要指定的位置，以此保存组件的内部状态不被丢失。

`<keep-alive>` 中被缓存的组件在被切换时不会执行 `mounted` 和 `unmounted` 生命周期钩子，取而代之的是会执行 `activated` 和 `deactivated` 生命周期钩子。

## Props

- `include` 接受字符串、正则或数组，只有名称匹配的组件才会缓存
- `exclude` 接受字符串、正则或数组，名称匹配的组件不会被缓存
- `max` 接受字符串和数值，控制组件缓存的数量阈值，超过阈值时会按激活的时间顺序，从最旧的缓存开始清理
