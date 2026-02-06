---
id: Interface
sidebar_position: 7
---

## Interface: `Transferable`

对象可以实现 `Transferable` 接口以创建自己的自定义可传输对象。这在传入或传出 worker 的对象包含深度嵌套的可传输对象（如 `ArrayBuffer` 或 `MessagePort`）时非常有用。

`Transferable` 对象公开两个由 Piscina 检查的属性，以确定如何传输对象。这些属性使用特殊的静态 `Piscina.transferableSymbol` 和 `Piscina.valueSymbol` 属性命名：

* `Piscina.transferableSymbol` 属性提供要包含在 `transferList` 中的对象（或对象）。

* `Piscina.valueSymbol` 属性提供一个代理值来代替 `Transferable` 本身进行传输。

两个属性都是必需的。

例如：

```js
const {
  move,
  transferableSymbol,
  valueSymbol
} = require('piscina');

module.exports = () => {
  const obj = {
    a: { b: new Uint8Array(5); },
    c: { new Uint8Array(10); },

    get [transferableSymbol]() {
      // Transfer the two underlying ArrayBuffers
      return [this.a.b.buffer, this.c.buffer];
    }

    get [valueSymbol]() {
      return { a: { b: this.a.b }, c: this.c };
    }
  };
  return move(obj);
};
```
