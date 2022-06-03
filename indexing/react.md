---
type: Indexing
tags: Frontend MVVM JavaScript QoK-B
---

# React

## React 官方文档

时隔几年重看 React 官方文档。这次从概念开始阅读。

需要解释的是，下面所有方便起见的 “React 提到” 这种描述方式实际上指代的是 “在 React 官方文档中提到”。

### 1. 官方对 React 对定位

官方对 React 的定义是一个用于构建 UI 的 JavaScript Library。原文如下：

> React is a JavaScript library for building user interfaces.

### 2. why JSX

React 认为渲染逻辑和 UI 逻辑是天然耦合在一起的。我们处理事件、处理状态变化和准备显示的数据的逻辑不应该分开。

与其将这些逻辑分散在不同文件，React 以组件为单位将两者结合在一块，还能保持一定松耦合。

**一些操作模版时的特性和差异（与 [[vue]] 的 template 对比）。**

- 在标签属性中使用表达式时不需要引号，引号直接代表字面量

```jsx
// 设置变量
const element = <img src={user.avatarUrl}></img>

// 引号表示字面量，不会编译
const element = <a href="https://www.reactjs.org"> link </a>
```

- JSX 更靠近 JavaScript，所以在标签属性等命名方式上使用驼峰命名，而非 HTML 规格的小些中划线命名

> 这一点与 Vue 完全不同，Vue 在尽量往 web component 规格上靠，所以建议遵从 HTML 规格在 template 中使用小些中划线对属性命名，即使这些属性在 js 代码中是按照驼峰形式来使用的。

HTML 中部分属性名称在 JavaScript 中是保留的关键字，这些属性需要使用别名来设置：class -> className、for -> htmlFor。

- JSX 会自动处理 XSS，经过编译的内容会被转义

> 这一点同 Vue 的 template 一样，相同两者都暴露一个属性来直接注入 HTML 代码。

- JSX 会被编译为对象

这两个写法的效果是一样的，可以理解上面的写法会被 Babel 编译为下面的形式。

```jsx
const element = <h1 className="greeting">Hello, world!</h1>
```

```jsx
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, world!'
)
```

经过 React 的编译器，这段代码会生成类型下面形式的对象，这是这个对象的简化形式方便理解。

```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!',
  },
}
```

### 3. React 中 Element 的定义

Element 在 React 中定义类似 DOM 元素。比如下面代码代表一个 React 元素。相对 DOM 元素，React 的元素创建成本更低，并且 React 会负责根据元素的变化更新实际的 DOM 元素。

> 就是虚拟 DOM。

```jsx
const element = <h1>Hello, world</h1>
```

React 特别提到 Element 和 Component 的区别在于，Element 是组成 Component 的部分。

**React 的 root 元素**

一个 root 元素表示一个 React 应用，使用下面方式创建并渲染 root 元素。

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'))
const element = <h1>Hello, world</h1>
root.render(element)
```

**React Element 是不可变的**

Element 在 React 中还有一个特性，首先 Immutable 是 React 的哲学，具体就体现在 Element 元素就是不可变的。React 描述 Element 就像电影的一帧画面，是定格的，唯一一种方式更新当前的 UI 就是创建一个新的 Element 并重新渲染，也就是调用 `root.render()` 函数。

> 这点同 Vue 是一致的，Vue 的 template 或者 render 函数返回一个对象，也是通过 diff 算法找出这个对象和目前已有对象的差异来执行更新操作的。

React 还提到在思考模式上考虑在这一刻 UI 应该怎么呈现，而不是如何改变状态，会消除绝大多数 bug。换句话说其实就是 [[concept-declarative|声明式]] 和 [[concept-imperative|命令式]] 的思维差异，在有框架处理繁琐细节之后，声明式开发会更少出现低级 bug。

但是哪个框架都无法做到完全的声明式开发，至少前端开发在事件处理上就不得不使用大量命令式逻辑来处理和转发事件。

### 4. 组件

React 定义组件类似函数，而元素类似对象，组件的作用在于接受任意参数（被称作 `props`）然后产生 React 元素描述屏幕上应该怎样展示。

React 组件的渲染方式和 React 元素相同，React 会通过名称来对两者区分处理。在 React 中组件使用大驼峰命名，而非大驼峰命名则会被视作 React 元素处理。

**组件的 props**

React 严格要求组件不允许修改 props 属性的值。

> 同 Vue 最大的不同在于 props 的访问方式。在 React 中函数式组件接受第一个参数作为 props 对象，取值的时候直接从这个对象获取，在 class 形式组件的写法中 props 被绑定在 `this` 上，我们通过 `this.props.xxx` 的方式来访问属性。在 Vue 中，所有 props 和 data 中的属性都被绑定在 `this` 上。
>
> 换句话说，在 React 中我们需要关系这个值来自 props 或者是 state；在 Vue 中则完全不关注值来源自哪里，统一动使用 `this` 获取。

**组件的 state**

React 要求我们不能直接修改 state 属性。所有针对 state 的修改操作要通过 `this.setState()` 方法来完成。并且一次更新中多个 `this.setState()` 调用会被收集到一块执行，这个机制会造成依赖 props 和 state 进行计算的时候没有获取到最新的值，要解决这个方法，需要使用回调函数来更新 state：`this.setState((state, props) => ({/* ... */}))`，在回调函数中，React 可以保证我们访问的 state 是最新状态。

> 与 Vue 最大不同之处在于，Vue 的赋值是立即执行的，但是副作用是放到宏任务中，等待当前宏任务中所有赋值操作结束之后一并执行，这种方式会避免相同副作用重复被触发造成性能问题。
>
> React 的做法是将 `this.setState()` 设定的数据收集起来最后合并到一块，仅执行一次更新操作。这会造成直觉上我们认为已经更新过的值实际在操作的时间点是未被更新的。对于响应式的属性被依赖的地方不会造成影响，因为一旦属性最终被更新了，所有依赖的位置都会进行重新计算，但是对于在 `this.setState()` 中依赖 props 和 state 进行计算的情况，需要把计算过程从同步计算变为使用回调函数异步计算，React 会在 state 对象合并完成之后，将最终结果作为回调函数的参数传递给我们的代码，以此来获取最新的状态进行正确的计算。
>
> 对比 React 和 Vue 的处理，两者遵从不同的理念实现的数据绑定处理，Vue 的方式或许更符合直觉。但换言之，更强的约束能产生质量更高的代码。Vue 灵活的方式反而拉低了代码质量的下限。

### 5. 事件处理

JSX 处理元素事件可以直接传递函数。与 HTML 不同之处在于 JSX 无法通过 `return false` 命令阻止事件的默认行为，要实现这一操作，需要在函数中执行合成事件对象的 `e.preventDefault()` 方法。

在使用 class 语法实现一个组件是，事件处理通常的做法是使用 class 的方法来处理对应的事件。但是要注意，JavaScript 的 class 中，方法不是自动绑定当前的 `this` 对象的，这一操作需要手动完成，如果你忘记绑定 `this` 到 class 的方法上，所有方法中针对 `this` 的操作都会抛出空指针。注意这个不是 React 定义的行为，这是 JavaScript 的行为。下面存在两种方式避免手动绑定 `this` 对象。

- class 字段语法可以保证 `this` 自动绑定，这个语法支持可能需要 babel 进行编译

```jsx
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this)
  }

  render() {
    return <button onClick={this.handleClick}>Click me</button>
  }
}
```

- 使用箭头函数自动绑定 `this` 为当前作用域

箭头函数的问题在于每次执行 render 函数的时候生成的箭头函数不是同一个内存地址，这会造成将箭头函数作为 props 传递给子组件的时候，每次重新执行 render 函数会导致子组件额外的渲染操作，这可能引起性能问题。

```jsx
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this)
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return <button onClick={() => this.handleClick()}>Click me</button>
  }
}
```

### 6. 表单

JSX 表单与 HTML 中对应元素稍有区别。原生元素比如 `<input>`、`<select>` 等在 JSX 中通过 `value` 属性传值，通过 `onchange` 方法调用 `this.setState()` 更新。

> 这一点 Vue 中原生元素与一版 HTML DOM 完全无异，降低学习成本。但 React 中 JSX 的处理是标准化的，一旦熟悉这种方式，其实相对处理逻辑各异的 HTML 元素来说，React 的方式更加方便。

### Refs

- [Getting Started – React](https://reactjs.org/docs/getting-started.html)

## JSX in Depth

JSX 基本上提供 `React.createElement(component, props, ...children)` 函数的语法糖。下面的 JSX 代码：

```jsx
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

编译为：

```js
React.createElement(MyButton, { color: 'blue', shadowSize: 2 }, 'Click Me')
```
