# How to count items in an array? 如何在数组中对元素计数

## 1. [[Array.prototype.filter]]

通过数组的 `filter` 方法筛选之后，获取结果数组的长度作为对指定元素的计数。

```js
const count = [
  /* array elements here */
].filter((item) => /* count conditions here */ item.id === "xxx").length;
```

## 2. [[Array.prototype.reduce]]

通过数组的 `reduce` 方法对指定元素计数。

```js
const count = [
  /* array elements here */
].reduce(
  // sum -> result sum, item -> current item
  (sum, item) => sum + (item.id === "xxx"),
  /* default sum */ 0
);
```
