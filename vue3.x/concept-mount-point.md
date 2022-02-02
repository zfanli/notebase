---
type: Vue
tags: Vue3.x
created: 2022-02-02
---

# Mount Point

非兼容。

Vue 2.x 中挂载应用会替换我们要挂载的元素。Vue 3.x 应用会插入挂载元素的 `innerHTML`，作为子元素添加进 DOM 树。

## Vue 2.x 行为

```js
new Vue({
  el: "#app",
  data() {
    return {
      message: "Hello Vue!",
    }
  },
  template: `
    <div id="rendered">{{ message }}</div>
  `,
})

// 或
const app = new Vue({
  data() {
    return {
      message: "Hello Vue!",
    }
  },
  template: `
    <div id="rendered">{{ message }}</div>
  `,
})

app.$mount("#app")
```

挂载点。

```html
<body>
  <div id="app">Some app content</div>
</body>
```

结果。应用替换了挂载点。

```html
<body>
  <div id="rendered">Hello Vue!</div>
</body>
```

## Vue 3.x 行为

```js
const app = Vue.createApp({
  data() {
    return {
      message: "Hello Vue!",
    }
  },
  template: `
    <div id="rendered">{{ message }}</div>
  `,
})

app.mount("#app")
```

挂载结果。 `#app` 被保留。

```html
<body>
  <div id="app" data-v-app="">
    <div id="rendered">Hello Vue!</div>
  </div>
</body>
```
