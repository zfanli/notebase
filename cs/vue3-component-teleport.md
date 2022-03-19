---
type: Vue3
# tags: Vue3.x
created: 2022-02-08
---

# Component: teleport

新增。

`<teleport>` 组件受到渲染器的底层支持，用于将模版传送至指定位置进行渲染和挂载。

`<teleport>` 组件的目的在于解决逻辑上完成同一个功能的组件，由于技术限制需要分散到不同组件的问题。比如，打开一个模态框的按钮组件，逻辑上按钮本身和模态框本体共同完成了一个功能，但是受限制于 CSS `position` 属性依赖父元素的位置，模态框本体需要放在 body 下才能正常全屏显示。

此时可以使用 `<teleport>` 组件将模态框部分模版渲染的结果挂载在 body 下面。

```html
<button @click="modalOpen = true">
  Open full screen modal! (With teleport!)
</button>

<teleport to="body">
  <div v-if="modalOpen" class="modal">
    <div>
      I'm a teleported modal! (My parent is "body")
      <button @click="modalOpen = false">Close</button>
    </div>
  </div>
</teleport>
```

这样模版渲染逻辑和按钮操作逻辑放在一起，可以访问同一个 `this` 下面的所有属性。并且不管实际上 `<teleport>` 内的模版在何处挂载，如果其中存在其他 Vue 组件，那么这些组件将成为当前组件逻辑上的子组件，在 Vue Devtools 中你会看到这些组件嵌套在当前组件下，即使实际上并不在当前组件中挂载。

`<teleport>` 可以对同一个元素进行多次挂载，挂载将按照执行的顺序进行。

## Props

### `to`

字符串。需要是有效的选择器。

### `disabled`

布尔值。这个属性用于禁用 `<teleport>` 功能。

禁用后 `<teleport>` 元素中的模版会在当前组件中定义的位置挂载并渲染。禁用和非禁用不会创建新的元素，而是将旧有元素移动到不同的挂载位置。已经渲染的内容的状态会被保留，这可以用来实现可局部预览，亦可全屏预览的内容组件，比如 PDF 预览或视频播放。
