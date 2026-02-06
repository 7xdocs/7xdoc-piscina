---
id: Events
sidebar_position: 4
---

## Event: `'error'`

当发生以下情况时，此类的实例会发出 `'error'` 事件：

- 未捕获的异常发生在当前不处理任务的 Worker 线程内。
- 从 Worker 线程发送了意外消息。

所有其他错误通过拒绝从 `run()` 或 `runTask()` 返回的 `Promise` 来报告，包括由处理程序函数本身报告的拒绝。

## Event: `'drain'`

每当 `queueSize` 达到 `0` 时，就会发出 `'drain'` 事件。

## Event: `'needsDrain'`

类似于 [`Piscina#needsDrain`](https://github.com/piscinajs/piscina#property-needsdrain-readonly)；一旦排队等待执行的任务数量超过池的总容量，就会触发此事件。

## Event: `'message'`

每当从 worker 线程收到消息时，就会发出 `'message'` 事件。

## Event: `'workerCreate'`

当创建新 worker 时触发的事件。

作为参数，它接收 worker 实例。

## Event: `'workerDestroy'`

当 worker 被销毁时触发的事件。

作为参数，它接收已被销毁的 worker 实例。
