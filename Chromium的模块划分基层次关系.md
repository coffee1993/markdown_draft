## Chromium的模块划分和层次关系

### 前言

参见：[How Chromium Displays Web Pages](https://www.chromium.org/developers/design-documents/displaying-a-web-page-in-chrome)

[Chromium网页加载过程简要介绍和学习计划](http://blog.csdn.net/luoshengyang/article/details/50414848)

官方文档How Chromium Displays Web的第一句话就是：
>This document describes how web pages are displayed in Chromium from the bottom up. Be sure you have read the multi-process architecture design document. You will especially want to understand the block diagram of major components. You may also be interested in multi-process resource loading for how pages are fetched from the network.

大致意思就是本文档描述了网页如何从Chormium的底层框架显示出来，需要提前阅读多进程架构设计文档。可以了解主要组件的框架结构，并知道多进程中如何从网络中提取资源加载的页面。

贴出来：[多进程架构官方文档 Multi-process Architecture](https://www.chromium.org/developers/design-documents/multi-process-architecture)

[多进程架构](http://blog.csdn.net/luoshengyang/article/details/47364477)
