---
id: Properties
sidebar_position: 5
---

## Property: `completed` (readonly)

当前完成的任务数。

## Property: `duration` (readonly)

自创建此 `Piscina` 实例以来的时长（以毫秒为单位）。

## Property: `options` (readonly)

当前由此实例使用的选项的副本。此对象具有与传递给构造函数的选项对象相同的属性。

## Property: `runTime` (readonly)

总结已完成任务的收集运行时间的直方图摘要对象。所有值均以毫秒表示。

* `runTime.average` {`number`} 所有任务的平均运行时间
* `runTime.mean` {`number`} 所有任务的平均（mean）运行时间
* `runTime.stddev` {`number`} 收集的运行时间的标准差
* `runTime.min` {`number`} 记录的最快运行时间
* `runTime.max` {`number`} 记录的最慢运行时间

所有遵循模式 `p{N}`（其中 N 是数字，例如 `p1`，`p99`）的属性表示运行时间观测值的百分位数分布。例如，`p99` 是第 99 个百分位数，表示 99% 的观测运行时间快于或等于给定值。

```js
{
  average: 1880.25,
  mean: 1880.25,
  stddev: 1.93,
  min: 1877,
  max: 1882.0190887451172,
  p0_001: 1877,
  p0_01: 1877,
  p0_1: 1877,
  p1: 1877,
  p2_5: 1877,
  p10: 1877,
  p25: 1877,
  p50: 1881,
  p75: 1881,
  p90: 1882,
  p97_5: 1882,
  p99: 1882,
  p99_9: 1882,
  p99_99: 1882,
  p99_999: 1882
}
```

## Property: `threads` (readonly)

此池使用的 `Worker` 实例的数组。

## Property: `queueSize` (readonly)

当前等待分配给 Worker 线程的任务数。

## Property: `needsDrain` (readonly)

指定提交的任务数量是否已超过池容量的布尔值。

此属性有助于决定对提交给池的任务数量施加背压。

## Property: `utilization` (readonly)

比较已完成任务的近似总平均运行时间与池的总运行时间容量的时间点比率。

池的运行时间容量是通过将 `duration` 乘以 `options.maxThread` 计数来确定的。这提供了池能够达到的绝对理论最大总计算时间。

近似总平均运行时间是通过将所有已完成任务的平均运行时间乘以已完成任务的总数来确定的。此数字表示池一直在积极处理任务的近似时间量。

然后通过将近似总平均运行时间除以容量来计算利用率，产生一个介于 `0` 和 `1` 之间的分数。

## Property: `waitTime` (readonly)

总结任务在队列中等待的收集时间的直方图摘要对象。所有值均以毫秒表示。

* `waitTime.average` {`number`} 所有任务的平均等待时间
* `waitTime.mean` {`number`} 所有任务的平均（mean）等待时间
* `waitTime.stddev` {`number`} 收集的等待时间的标准差
* `waitTime.min` {`number`} 记录的最快等待时间
* `waitTime.max` {`number`} 记录的最长等待时间

所有遵循模式 `p{N}`（其中 N 是数字，例如 `p1`，`p99`）的属性表示等待时间观测值的百分位数分布。例如，`p99` 是第 99 个百分位数，表示 99% 的观测等待时间快于或等于给定值。

```js
{
  average: 1880.25,
  mean: 1880.25,
  stddev: 1.93,
  min: 1877,
  max: 1882.0190887451172,
  p0_001: 1877,
  p0_01: 1877,
  p0_1: 1877,
  p1: 1877,
  p2_5: 1877,
  p10: 1877,
  p25: 1877,
  p50: 1881,
  p75: 1881,
  p90: 1882,
  p97_5: 1882,
  p99: 1882,
  p99_9: 1882,
  p99_99: 1882,
  p99_999: 1882
}
```
