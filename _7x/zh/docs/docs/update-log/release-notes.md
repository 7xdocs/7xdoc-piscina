---
id: Release Notes
sidebar_position: 1
---

### 4.1.0

#### 特性

* 添加 `needsDrain` 属性 ([#368](https://github.com/piscinajs/piscina/issues/368)) ([2d49b63](https://github.com/piscinajs/piscina/commit/2d49b63368116c172a52e2019648049b4d280162))
* 正确处理任务外部的 process.exit 调用 ([#361](https://github.com/piscinajs/piscina/issues/361)) ([8e6d16e](https://github.com/piscinajs/piscina/commit/8e6d16e1dc23f8bb39772ed954f6689852ad435f))


#### 错误修复

* 修复 TypeScript 4.7 的类型 ([#239](https://github.com/piscinajs/piscina/issues/239)) ([a38fb29](https://github.com/piscinajs/piscina/commit/a38fb292e8fcc45cc20abab8668f82d908a24dc0))
* 使用 CJS 导入 ([#374](https://github.com/piscinajs/piscina/issues/374)) ([edf8dc4](https://github.com/piscinajs/piscina/commit/edf8dc4f1a19e9b49e266109cdb70d9acc86f3ca))

### 4.0.0

* 放弃 Node.js 14.x 支持
* 将 Node.js 20.x 添加到 CI

### 3.2.0

* 添加一个新的 `PISCINA_DISABLE_ATOMICS` 环境变量，作为禁用 Piscina 内部使用 `Atomics` API 的另一种方式。(https://github.com/piscinajs/piscina/pull/163)
* 修复了可转移对象的错误。(https://github.com/piscinajs/piscina/pull/155)
* 修复了 TypeScript 的 CI 问题。(https://github.com/piscinajs/piscina/pull/161)

### 3.1.0

* 弃用 `piscina.runTask()`；添加 `piscina.run()` 作为替代方案。
  https://github.com/piscinajs/piscina/commit/d7fa24d7515789001f7237ad6ae9ad42d582fc75
* 允许从单个文件导出多个处理程序函数。
  https://github.com/piscinajs/piscina/commit/d7fa24d7515789001f7237ad6ae9ad42d582fc75

### 3.0.0

* 放弃 Node.js 10.x 支持
* 将最低 TypeScript 目标更新为 ES2019

### 2.1.0

* 添加 name 属性以指示在使用 `AbortController`（或类似物）取消任务时的 `AbortError`
* 更多示例

### 2.0.0

* 添加了非托管文件描述符跟踪
* 更新了依赖项

### 1.6.1

* 错误修复：如果 AbortSignal 已中止，则拒绝
* 错误修复：对中止事件使用一次性监听器

### 1.6.0

* 添加 `niceIncrement` 配置参数。

### 1.5.1

* 围绕可中止任务选择的错误修复。

### 1.5.0

* 添加了 `Piscina.move()`
* 添加了自定义任务队列
* 添加了利用率指标
* 在将 worker 视为候选者之前等待它们准备就绪
* 其他示例

### 1.4.0

* 添加了 `maxQueue = 'auto'` 以自动计算最大队列大小。
* 添加了更多示例，包括将 worker 实现为 Node.js 原生插件的示例。

### 1.3.0

* 添加了 `'drain'` 事件

### 1.2.0

* 添加了对 ESM 和 file:// URL 的支持
* 添加了 `env`、`argv`、`execArgv` 和 `workerData` 选项
* 更多示例

### 1.1.0

* 添加了对 Worker 线程 `resourceLimits` 的支持

### 1.0.0

* 首次发布！
