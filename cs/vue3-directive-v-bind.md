---
type: Vue3
# tags: Vue3.x
created: 2022-02-01
---

# Directive: v-bind

## Merge Behavior

非兼容。

有时会出现下面用法。通过独立属性和 `v-bind="object"` 写法定义同一个属性，Vue 2.x 时独立属性会一直覆盖 `v-bind="object"` 的定义。

```html
<!-- 模板 -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- 结果 -->
<div id="red"></div>
```

Vue 3.x 根据属性定义的顺序决定覆盖结果，后面定义的属性会覆盖前面的同名属性。

```html
<!-- 模板 -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- 结果 -->
<div id="blue"></div>

<!-- 模板 -->
<div v-bind="{ id: 'blue' }" id="red"></div>
<!-- 结果 -->
<div id="red"></div>
```
