---
type: Vue3
---

# Component: transition

`<transition>` 组件受到渲染器的底层支持，可以给其 default 插槽的组件装载挂载和卸载过渡动画。`<transition>` 组件实际上只是给默认插槽的组件绑定上过渡钩子，以通知渲染器在挂载和卸载时采取不同的处理。过渡钩子在组件挂载前后、以及卸载前后留出操作空间，默认使用 class 完成 CSS 过渡动画，但是可以根据选项关闭这个行为，变成只通过发射事件通知 JavaScript 钩子。

## 参考

- https://v3.cn.vuejs.org/api/built-in-components.html#transition
