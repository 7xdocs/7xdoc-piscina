---
sidebar_position: 1
slug: /
---

# 简介

Piscina.js 是一个强大的 Node.js 工作线程池库，允许你使用工作线程高效地并行运行 CPU 密集型任务。它提供了一个简单的 API，用于将计算密集型任务卸载到工作线程池中，从而提高 Node.js 应用程序的性能和可扩展性。

## 为什么选择 Piscina？

在 worker threads（工作线程）的早期，Node.js 核心团队遇到过一个问题，用户的应用程序启动了数千个并发工作线程，导致性能问题。虽然这个具体问题有助于发现 worker 实现中的一个轻微内存泄漏，但它突显了一个更广泛的问题：由于缺乏理解而滥用 worker threads。

虽然 worker threads 已经成熟并且其使用变得更加普遍，但仍然需要更好的示例和关于其正确使用的教育。这一认识促成了 Piscina 的创建，这是一个由 [NearForm Research](https://www.nearform.com/) 赞助的开源项目，专注于为在 Node.js 应用程序中使用 worker threads 提供指导和最佳实践。

随着 worker threads 现在成为 Node.js 中一个完善的功能，Piscina 旨在弥合 worker threads 的潜力与其具体实现之间的差距。

## 关键特性

✔ 线程间的快速通信\
✔ 涵盖固定任务和可变任务场景\
✔ 支持灵活的池大小\
✔ 正确的 async tracking 集成\
✔ 运行和等待时间的统计跟踪\
✔ 支持取消\
✔ 支持强制内存资源限制\
✔ 支持 CommonJS、ESM 和 TypeScript\
✔ 自定义任务队列\
✔ Linux 上的可选 CPU 调度优先级

## 赞助商

想要支持 Piscina 的开发吗？考虑在 [Open Collective](https://opencollective.com/piscinajs) 上赞助我们。我们感谢所有级别的支持！

### 铜牌赞助商

<a href="https://testmu.ai/?utm_source=piscinajs&utm_medium=sponsor" target="_blank" rel="noopener noreferrer">
<img src="https://assets.testmu.ai/resources/images/logos/black-logo.png" alt="TestMu.ai logo" style={{ verticalAlign: "middle" }} width="250" height="110" className="img-dark"/>
<img src="https://assets.testmu.ai/resources/images/logos/white-logo.png" alt="TestMu.ai logo" style={{ verticalAlign: "middle" }} width="250" height="110" className="img-light"/>
</a>
