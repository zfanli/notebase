---
type: StateManagement
tags: StateManagement Vue JavaScript
---

# Pinia: State

State 可视作 [[vue-pinia-store|store]] 的 `data` 选项，同样它需要一个函数来生成默认的状态。

State 的属性默认可以直接读写。

```js
const store = useStore()

store.counter++
```

State 可以使用 `$reset()` 方法进行重置，state 会恢复到初始状态。

```js
const store = useStore()

store.$reset()
```

State 可以通过 `$patch()` 方法来同时修改多个属性，这个方法接受对象或函数来修改 state。这个方法最大的用处在于将一组 mutation 关联到开发工具上。

```js
store.$patch({
  counter: store.counter + 1,
  name: 'John',
})

cartStore.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})
```

State 可以被整个替换掉。

```js
store.$state = { counter: 666, name: 'John' }
```

State 可以被监听。`$subscribe()` 方法提供与 Vuex 类似的功能。`$subscribe()` 与常规 Vue 3.x 的 [[vue3-global-api-watch|watch()]] 的区别在于执行的时机，`$subscribe()` 只会在补丁更新之后执行一次。通过 `$subscribe()` 订阅的回调函数默认会和组件绑定（使用 `setup` 时），这表示在组件卸载时这些订阅回调函数会被移除。如果你需要订阅函数不和组件绑定，需要在第二个参数指定 `{ detached: true }` 来告诉 Pinia 在卸载组件时不对其进行移除。

```js
// $subscribe()
cartStore.$subscribe((mutation, state) => {
  // import { MutationType } from 'pinia'
  mutation.type // 'direct' | 'patch object' | 'patch function'
  // same as cartStore.$id
  mutation.storeId // 'cart'
  // only available with mutation.type === 'patch object'
  mutation.payload // patch object passed to cartStore.$patch()

  // persist the whole state to the local storage whenever it changes
  localStorage.setItem('cart', JSON.stringify(state))
})

// { detached: true }
export default {
  setup() {
    const someStore = useSomeStore()

    // this subscription will be kept after the component is unmounted
    someStore.$subscribe(callback, { detached: true })

    // ...
  },
}
```
