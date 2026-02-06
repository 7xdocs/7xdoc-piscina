---
id: Methods
sidebar_position: 3
---
## Method: `run(task[, options])`

调度要在 Worker 线程上运行的任务。

* `task`: 任何值。这将传递给从 `filename` 导出的函数。
* `options`:
  * `transferList`: 可选的对象列表，当将 `task` 发布到 Worker 时传递给 [`postMessage()`]，这些对象是被传输而不是克隆。
  * `filename`: 可选地覆盖此任务传递给构造函数的 `filename` 选项。如果构造函数未指定 `filename`，则这是强制性的。
  * `name`: 可选地覆盖用于任务的导出 worker 函数。
  * `abortSignal`: 一个 `AbortSignal` 实例。如果传递，这可用于取消任务。如果任务已经在运行，相应的 `Worker` 线程将被停止。
    （更一般地说，任何发出 `'abort'` 事件的 `EventEmitter` 或 `EventTarget` 都可以传递在这里。）无论 `concurrentTasksPerWorker` 选项如何，可中止任务都不能共享线程。

这返回一个 `Promise`，用于从 `filename` 导出的函数的（异步）函数调用的返回值。如果（异步）函数抛出错误，返回的 `Promise` 将被拒绝并带有该错误。
如果任务被中止，返回的 `Promise` 也会被拒绝并带有错误。

## Method: `runTask(task[, transferList][, filename][, abortSignal])`

**已弃用** -- 请改用 `run(task, options)`。

调度要在 Worker 线程上运行的任务。

* `task`: 任何值。这将传递给从 `filename` 导出的函数。
* `transferList`: 可选的对象列表，当将 `task` 发布到 Worker 时传递给 [`postMessage()`]，这些对象是被传输而不是克隆。
* `filename`: 可选地覆盖此任务传递给构造函数的 `filename` 选项。如果构造函数未指定 `filename`，则这是强制性的。
* `signal`: 一个 [`AbortSignal`][] 实例。如果传递，这可用于取消任务。如果任务已经在运行，相应的 `Worker` 线程将被停止。
  （更一般地说，任何发出 `'abort'` 事件的 `EventEmitter` 或 `EventTarget` 都可以传递在这里。）无论 `concurrentTasksPerWorker` 选项如何，可中止任务都不能共享线程。

这返回一个 `Promise`，用于从 `filename` 导出的函数的（异步）函数调用的返回值。如果（异步）函数抛出错误，返回的 `Promise` 将被拒绝并带有该错误。
如果任务被中止，返回的 `Promise` 也会被拒绝并带有错误。

## Method: `destroy()`

停止所有 Worker 并拒绝所有待处理任务的 `Promise`。

这返回一个 `Promise`，一旦所有线程都停止，该 Promise 就会被实现。

## Method: `close([options])`

* `options`:
  * `force`: 一个 `boolean` 值，指示是否中止所有已排队但尚未开始的任务。默认为 `false`。

它优雅地停止所有 Worker。

这返回一个 `Promise`，一旦所有已开始的任务都已完成且所有线程都已停止，该 Promise 就会被实现。

此方法类似于 `destroy()`，不同之处在于 `close()` 将等待 worker 任务完成，而 `destroy()` 将立即中止它们。
