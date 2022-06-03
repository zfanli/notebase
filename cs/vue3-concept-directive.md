---
type: Vue
tags: Vue JavaScript QoK-B
created: 2022-02-02
---

# Concept: directive

## 自定义指令

非兼容。

指令的钩子被重命名，以保持和组件生命周期一致。且以前 `binding` 的属性 `expression` 不再传递。

### Vue 2.x 用法

2.x 定义了下面这些钩子。

- **bind** - 指令绑定到元素后调用。只调用一次。
- **inserted** - 元素插入父 DOM 后调用。
- **update** - 当元素更新，但子元素尚未更新时，将调用此钩子。
- **componentUpdated** - 一旦组件和子级被更新，就会调用这个钩子。
- **unbind** - 一旦指令被移除，就会调用这个钩子。也只调用一次。

用例：

```html
<p v-highlight="'yellow'">以亮黄色高亮显示此文本</p>
```

指令定义：

```js
Vue.directive('highlight', {
  bind(el, binding, vnode) {
    el.style.background = binding.value
  },
})
```

### Vue 3.x 用法

3.x 指令钩子的名称修改为与组件生命周期一致，且添加了部分钩子。

- **created** - 新增！在元素的 attribute 或事件监听器被应用之前调用。
- bind → **beforeMount**
- inserted → **mounted**
- **beforeUpdate**：新增！在元素本身被更新之前调用，与组件的生命周期钩子十分相似。
- ~~update~~ → 移除！该钩子与 updated 有太多相似之处，因此它是多余的。请改用 updated。
- componentUpdated → **updated**
- **beforeUnmount**：新增！与组件的生命周期钩子类似，它将在元素被卸载之前调用。
- unbind -> **unmounted**

```js
const MyDirective = {
  created(el, binding, vnode, prevVnode) {}, // 新增
  beforeMount() {},
  mounted() {},
  beforeUpdate() {}, // 新增
  updated() {},
  beforeUnmount() {}, // 新增
  unmounted() {},
}
```

上面 `highlight` 指令的例子在 3.x 写法：

```js
const app = Vue.createApp({})

app.directive('highlight', {
  beforeMount(el, binding, vnode) {
    el.style.background = binding.value
  },
})
```

### 边界情况：访问组件实例

> 如果需要依赖组件实例，那么应该考虑指令是否应该转换为组件。

2.x 的写法。

```js
bind(el, binding, vnode) {
  const vm = vnode.context
}
```

3.x 移动了组件实例绑定的位置。

```js
mounted(el, binding, vnode) {
  const vm = binding.instance
}
```
