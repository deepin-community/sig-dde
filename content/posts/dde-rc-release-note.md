---
title: "DDE v23 beta3 发布说明"
date: 2024-05-15T17:00:00+08:00
draft: false
authors: ["BLumia"]
tags: ["成果公示"]
---

deepin v23 RC 将于 2024 年 5 月 15 日发布，这里为大家简要描述本次更新中，DDE 所涉及的变更，以及我们的进一步计划。

需注意，本文章倾向于对 DDE 项目整体的技术内容进行描述，面向 DDE 开发者和对 DDE 开发感兴趣的读者，并非面向最终用户的特性概览文章。另外，如果你对 DDE 的移植感兴趣，请参阅[另一篇侧重于移植相关事项的文章](https://deepin-community.github.io/sig-dde-porting/posts/dde-v23rc-porting-guide/)。

<!--more-->

## 变化较大的默认组件

相比 deepin v23 beta3 而言，在于 deepin v23 RC 发布的 DDE 中，有一个项目得到了较大规模的重构，并随 RC 默认提供给各位用户。即：

- dde-shell

dde-shell 旨在提供一个桌面环境级的外壳程序，使编写 DDE 桌面组件变得更轻松。这个项目在 beta3 阶段为技术预览状态，而现阶段，dde-shell 则走出了技术预览，并取代了原本的 dde-dock 项目来展示任务栏组件。

此外，需要注意的是，dde-launchpad 项目也转而默认提供 dde-shell 插件的形式来提供启动器组件。

## 技术预览组件

*非技术用户请慎重启用技术预览功能*

deepin v23 beta2 时，我们提供了一个需要用户手动安装的 dcc-insider-plugin 插件，称为技术预览插件。这个组件旨在帮助用户方便的测试 deepin 未来版本中计划提供但仍不稳定的系统组件。需要注意的是，为了加快版本迭代速度，使 v23 首个稳定版可以更快面向用户发布，故这个组件在 RC 阶段并未进行过较多测试，因而可能不会按预期行为工作，故我们目前不建议您使用此插件。

- treeland / ddm
- deepin-im

treeland 与 deepin-im 这两个组件在 beta3 到 RC 的阶段中并无太大显著变化。其中，treeland 项目原本提供了合成器与 DM 两个功能，而现在，treeland 与 DM 也进行了拆分，后者被拆分到了 ddm 仓库之中。

## 后续计划

如之前计划所安排，dde-shell 现已走出技术预览，但 dde-shell 仍有很多需要进一步完善的地方。一些用户会注意到 dde-shell 版的任务栏缺失了一些原本 dde-dock 项目所提供的功能，此类特性会后续逐步补充上来。另外 dde-shell 也会继续向原本的目标迈进，我们会进一步将其他桌面组件转换为 dde-shell 插件的形式进行维护。使得桌面环境模块化之余，也为 wayland 支持作出更好的准备。

另外，由于 dde-shell 的初衷之一是使桌面环境更加模块化，我们也欢迎社区开发者开始尝试使用 dde-shell 编写一些自己觉得有趣的东西，并与我们讨论对 shell 相关设计与接口的体验，一同完善 dde-shell。如果你对这相关的话题感兴趣，欢迎加入 DDE SIG 的 Matrix 群聊 (`#dde:matrix.org`) 之中来。

接下来，wayland 会话支持也会变成主要目标（尽管 v23 首个正式版仍然可能不会提供 wayland 会话支持），treeland 与 ddm 将会逐步继续完善，并在恰当的时机提供给大家。

此外，由于 DDE 在 beta3 与 RC 的变化仍然较大，我们也仍然和上次一样提供了一篇移植注意事项博客。笔者也是 DDE 移植 SIG 的成员，对应的文章现已发布到 DDE 移植小组。如果您感兴趣，可[在此阅读](https://deepin-community.github.io/sig-dde-porting/posts/dde-v23rc-porting-guide/)。如果你本身在参与 DDE 的移植工作，那么也欢迎你加入 DDE 移植小组（`#dde-port:deepin.org` 或 `https://t.me/ddeport`）。

最后，感谢你读到这里。如有任何问题，欢迎在我们的开发者群（`#deepin-community:deepin.org`）进行讨论。

