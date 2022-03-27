---
type: StateManagement
tags: StateManagement Vue JavaScript
---

# Pinia: Actions

Action 等价于 store 的方法，设计用来存放业务逻辑。Action 中可以同 [[vue-pinia-getters|getters]] 一样通过 `this` 关键字访问整个 [[vue-pinia-store|store]] 对象。Action 可以是异步的，你可以在其中使用 `await` 关键字。

并且，action 可以自定义任何参数和返回值，action 调用时编辑器可以进行类型推测，提供更友好的开发体验。

```js
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  actions: {
    increment() {
      this.counter++
    },
    randomizeCounter() {
      this.counter = Math.round(100 * Math.random())
    },
  },
})

// 异步
import { mande } from 'mande'

const api = mande('/api/users')

export const useUsers = defineStore('users', {
  state: () => ({
    userData: null,
    // ...
  }),

  actions: {
    async registerUser(login, password) {
      try {
        this.userData = await api.post({ login, password })
        showTooltip(`Welcome back ${this.userData.name}!`)
      } catch (error) {
        showTooltip(error)
        // let the form component display the error
        return error
      }
    },
  },
})
```

Action 中导入其他的 store，即可调用其他 store 的 action。

```js
import { useAuthStore } from './auth-store'

export const useSettingsStore = defineStore('settings', {
  state: () => ({
    preferences: null,
    // ...
  }),
  actions: {
    async fetchUserPreferences() {
      const auth = useAuthStore()
      if (auth.isAuthenticated) {
        this.preferences = await fetchPreferences()
      } else {
        throw new Error('User must be authenticated')
      }
    },
  },
})
```

Action 可以通过 `$onAction()` 方法进行订阅。订阅的回调会在 action 执行之前调用，通过参数中的 `after` 和 `onError` 属性，可以在 action 调用之后、action 报错时进行相应处理。`$onAction()` 的返回值是一个清理函数，可以用来移除指定的订阅回调函数。`$onAction()` 同 [[vue-pinia-state|state]] 的 `$subscribe()` 一样默认同组件进行绑定，在组件卸载时进行移除，你可以通过指定第二个参数为 `true` 来阻止组件卸载时的移除行为。

```js
const unsubscribe = someStore.$onAction(
  ({
    name, // name of the action
    store, // store instance, same as `someStore`
    args, // array of parameters passed to the action
    after, // hook after the action returns or resolves
    onError, // hook if the action throws or rejects
  }) => {
    // a shared variable for this specific action call
    const startTime = Date.now()
    // this will trigger before an action on `store` is executed
    console.log(`Start "${name}" with params [${args.join(', ')}].`)

    // this will trigger if the action succeeds and after it has fully run.
    // it waits for any returned promised
    after((result) => {
      console.log(
        `Finished "${name}" after ${
          Date.now() - startTime
        }ms.\nResult: ${result}.`
      )
    })

    // this will trigger if the action throws or returns a promise that rejects
    onError((error) => {
      console.warn(
        `Failed "${name}" after ${Date.now() - startTime}ms.\nError: ${error}.`
      )
    })
  }
)

// manually remove the listener
unsubscribe()

// 阻止自动移除
export default {
  setup() {
    const someStore = useSomeStore()

    // this subscription will be kept after the component is unmounted
    someStore.$onAction(callback, true)

    // ...
  },
}
```
