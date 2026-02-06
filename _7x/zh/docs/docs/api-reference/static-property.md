---
id: Static Properties and Methods
sidebar_position: 6
---

## Static property: `isWorkerThread` (readonly)

如果此代码作为 Worker 在 `Piscina` 线程池内运行，则为 `true`。

## Static property: `version` (readonly)

提供此库的当前版本作为 semver 字符串。

## Static method: `move(value)`

默认情况下，worker 函数返回的任何值在返回给 Piscina 池时都会被克隆，即使该对象能够被传输。`Piscina.move()` 方法可用于包装和标记可传输值，以便它们被传输而不是克隆。

`value` 可以是 Node.js 支持为可传输的任何对象（例如 `ArrayBuffer`、任何 `TypedArray` 或 `MessagePort`），或任何实现 `Transferable` 接口的对象。

```js
const { move } = require('piscina');

module.exports = () => {
  return move(new ArrayBuffer(10));
}
```

如果 `value` 不可传输，`move()` 方法将抛出异常。

`move()` 方法返回的对象不应设置为对象中的嵌套值。如果使用它，`move()` 对象本身将被克隆，而不是传输它包装的对象。
