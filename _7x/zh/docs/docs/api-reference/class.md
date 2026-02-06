---
id: Instance
sidebar_position: 2
---

## Class: `Piscina`

Piscina 的工作原理是创建一个 Node.js 工作线程池，可以向其分派一个或多个任务。每个工作线程执行在一个单独文件中定义的单个导出函数。每当任务被分派给 worker 时，worker 调用导出的函数，并在函数完成时将返回值报告回 Piscina。

此类扩展了 Node.js 的 [`EventEmitter`](https://nodejs.org/api/events.html)。

### Constructor: `new Piscina([options])`

- 支持以下可选配置：

  - `filename`: (`string | null`) 提供在 Worker 线程上运行任务的代码的默认源。这应该是绝对路径或绝对 `file://` URL，指向一个导出 JavaScript `function` 或 `async function` 作为其默认导出或 `module.exports` 的文件。[ES modules](https://nodejs.org/api/esm.html) 也受支持。
  - `name`: (`string | null`) 提供默认导出的 worker 函数的名称。默认为 `'default'`，表示 worker 模块的默认导出。
  - `minThreads`: (`number`) 设置此线程池始终运行的最小线程数。默认值为 [`os.availableParallelism`](https://nodejs.org/api/os.html#osavailableparallelism) 提供的值。
  - `maxThreads`: (`number`) 设置此线程池运行的最大线程数。默认值为 [`os.availableParallelism`](https://nodejs.org/api/os.html#osavailableparallelism) \* 1.5 提供的值。
  - `idleTimeout`: (`number`) 以毫秒为单位的超时，指定 `Worker` 在关闭之前允许空闲（即不处理任何任务）多长时间。默认情况下，这是立即的。如果传递 `Infinity` 作为值，`Worker` 永远不会关闭。
    :::info
    默认的 `idleTimeout` 可能会导致应用程序的一些性能损失，因为涉及到停止和启动新工作线程的开销。为了提高性能，请尝试显式设置 `idleTimeout`。
    :::
    :::info
    当将 `idleTimeout` 设置为 `Infinity` 时要小心，因为这将阻止 worker 关闭，即使在空闲时也是如此，可能会导致资源过度使用。
    :::
  - `maxQueue`: (`number` | `string`) 在给定时间可能被调度运行但由于缺乏可用线程而尚未运行的最大任务数。默认情况下，没有限制。可以使用特殊值 `'auto'` 让 Piscina 计算最大值为 `maxThreads` 的平方。当使用 `'auto'` 时，可以通过检查 [`options.maxQueue`](#property-options-readonly) 属性找到计算出的 `maxQueue` 值。
  - `concurrentTasksPerWorker`: (`number`) 指定多少个任务可以同时共享单个 Worker 线程。默认为 `1`。通常只有在任务有某种异步组件时才有意义指定。请记住，Worker 线程通常不是为并行处理 I/O 而构建的。
  - `atomics`: (`sync` | `async` | `disabled`) 使用 [`Atomics`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics) API 在线程之间进行更快的通信。这是默认开启的。你可以通过设置环境变量 `PISCINA_DISABLE_ATOMICS` 为 `1` 来全局禁用 `Atomics`。
    如果 `atomics` 为 `sync`，它将导致在任务之间暂停线程（停止所有执行）。理想情况下，线程应在将控制权返回给主线程之前等待所有操作完成（避免在线程内有打开的句柄）。如果仍然希望有可能拥有打开的句柄或处理异步任务，你可以设置环境变量 `PISCINA_ENABLE_ASYNC_ATOMICS` 为 `1` 或设置 `options.atomics` 为 `async`。

    :::info
    **注意**：`async` 模式会带来性能损失，并且如果未正确跟踪打开的句柄，可能会导致意外行为。
    如果仍有任何后台操作正在运行，Workers 应该设计为在将控制权返回给主线程之前等待所有操作完成。
    `async` 可能有帮助（例如用于缓存预热等）。
    :::

  - `resourceLimits`: (`object`) 参见 [Node.js new Worker options](https://nodejs.org/api/worker_threads.html#worker_threads_new_worker_filename_options)
    - `maxOldGenerationSizeMb`: (`number`) 每个 worker 线程主堆的最大大小（以 MB 为单位）。
    - `maxYoungGenerationSizeMb`: (`number`) 最近创建的对象的堆空间的最大大小。
    - `codeRangeSizeMb`: (`number`) 用于生成代码的预分配内存范围的大小。
    - `stackSizeMb` : (`number`) 线程的默认最大堆栈大小。较小的值可能会导致不可用的 Worker 实例。默认值：4
  - `env`: (`object`) 如果设置，指定 worker 线程内 `process.env` 的初始值。详情请参见 [Node.js new Worker options](https://nodejs.org/api/worker_threads.html#worker_threads_new_worker_filename_options)。
  - `argv`: (`any[]`) 将被字符串化并附加到 worker 中 `process.argv` 的参数列表。详情请参见 [Node.js new Worker options](https://nodejs.org/api/worker_threads.html#worker_threads_new_worker_filename_options)。
  - `execArgv`: (`string[]`) 传递给 worker 的 Node.js CLI 选项列表。详情请参见 [Node.js new Worker options](https://nodejs.org/api/worker_threads.html#worker_threads_new_worker_filename_options)。
  - `workerData`: (`any`) 任何可以被克隆并作为 `require('piscina').workerData` 可用的 JavaScript 值。详情请参见 [Node.js new Worker options](https://nodejs.org/api/worker_threads.html#worker_threads_new_worker_filename_options)。与常规 Node.js Worker 线程不同，`workerData` 必须不指定任何需要 `transferList` 的值。这是因为 `workerData` 将为每个池化 worker 克隆。
  - `taskQueue`: (`TaskQueue`) 默认情况下，Piscina 对提交的任务使用先进先出队列。`taskQueue` 选项可用于提供替代实现。有关更多详细信息，请参阅 [Custom Task Queues](https://github.com/piscinajs/piscina#custom_task_queues)。
  - `niceIncrement`: (`number`) 一个可选值，用于降低单个线程的优先级，即值越高，Worker 线程的优先级越低。此值用于 Unix/Windows，需要安装可选的 [`@napi-rs/nice`](https://npmjs.org/package/@napi-rs/nice) 模块。有关更多详细信息，请参阅 [`nice(2)`](https://linux.die.net/man/2/nice) 和 [`SetThreadPriority`](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadpriority)。
  - `trackUnmanagedFds`: (`boolean`) 一个可选设置，当为 `true` 时，将导致 Workers 跟踪使用 `fs.open()` 和 `fs.close()` 管理的文件描述符，并在 Worker 退出时自动关闭它们。默认为 `true`。（此选项仅在 Node.js 12.19+ 和所有高于 14.6.0 的 Node.js 版本上受支持）。
  - `closeTimeout`: (`number`) 调用 `close()` 时等待池完成所有正在进行的任务的可选时间（以毫秒为单位）。默认值为 `30000`。
  - `recordTiming`: (`boolean`) 默认情况下，将记录池的运行和等待时间。要禁用，请设置为 `false`。
  - `workerHistogram`: (`boolean`) 默认值为 `false`。它将提示 Worker 池记录每个单独 Worker 的统计信息。
  - `loadBalancer`: ([`PiscinaLoadBalancer`](#piscinaloadbalancer)) 默认情况下，Piscina 使用最不繁忙算法。`loadBalancer` 选项可用于提供替代实现。有关更多详细信息，请参阅 [Custom Load Balancers](../advanced-topics/loadbalancer.mdx)。

:::caution
  设置资源限制时要小心。设置得太低的限制可能会导致 `Piscina` 工作线程无法使用。
:::

:::info
  **关于显式资源管理的说明**：Piscina 支持 `Symbol.dispose` 和 `Symbol.asyncDispose` 用于使用 `using` 关键字进行显式资源管理。
  这仅在 Node.js 24 及更高版本上可用。

  有关更多信息，请参阅 [Explicit Resource Management](https://github.com/tc39/proposal-explicit-resource-management)。
:::

## `PiscinaHistogram`

`PiscinaHistogram` 允许你访问工作线程池的直方图数据。
如果需要清除数据，可以根据请求重置它。

**Example**:

```js
import { Piscina } from 'piscina';

const pool = new Piscina({
  filename: resolve(__dirname, 'path/to/worker.js'),
});

const firstBatch = [];

for (let n = 0; n < 10; n++) {
  firstBatch.push(pool.run('42'));
}

await Promise.all(firstBatch);

console.log(pool.histogram.runTime); // Print run time histogram summary
console.log(pool.histogram.waitTime); // Print wait time histogram summary

// If in need to reset the histogram data for a new set of tasks
pool.histogram.resetRunTime();
pool.histogram.resetWaitTime();

const secondBatch = [];

for (let n = 0; n < 10; n++) {
  secondBatch.push(pool.run('42'));
}

await Promise.all(secondBatch);

// The histogram data will only contain the data for the second batch of tasks
console.log(pool.histogram.runTime);
console.log(pool.histogram.waitTime);
```

### Interface: `PiscinaHistogram`

- `runTime`: (`PiscinaHistogramSummary`) 运行时间直方图摘要。执行任务所需的时间。
- `waitTime`: (`PiscinaHistogramSummary`) 等待时间直方图摘要。任务提交和任务开始运行之间的时间。

> **注意**：仅当 `recordTiming` 设置为 `true` 时，直方图数据才可用。

```ts
type PiscinaHistogram = {
  runTime: PiscinaHistogramSummary;
  waitTime: PiscinaHistogramSummary;
  resetRunTime(): void; // Reset Run Time Histogram
  resetWaitTime(): void; // Reset Wait Time Histogram
}
```

### Interface: `PiscinaHistogramSummary`

```ts
type PiscinaHistogramSummary = {
  average: number;
  mean: number;
  stddev: number;
  min: number;
  max: number;
  p0_001: number;
  p0_01: number;
  p0_1: number;
  p1: number;
  p2_5: number;
  p10: number;
  p25: number;
  p50: number;
  p75: number;
  p90: number;
  p97_5: number;
  p99: number;
  p99_9: number;
  p99_99: number;
  p99_999: number;
}
```
