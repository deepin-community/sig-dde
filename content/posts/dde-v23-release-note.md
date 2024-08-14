---
title: "DDE v23 发布说明"
date: 2024-08-14T19:00:00+08:00
draft: false
authors: ["BLumia"]
tags: ["成果公示"]
---

deepin v23 将于 2024 年 8 月 15 日发布，这里为大家简要描述本次更新中，DDE 所涉及的变更，以及我们的进一步计划。

需注意，本文章是站在 DDE SIG 角度的，倾向于对 DDE 项目整体的技术内容进行描述，面向 DDE 开发者和对 DDE 开发感兴趣的读者，并非面向最终用户的特性概览文章。若您需要大众化的发布概览，请参阅 deepin 公众号、官方网站等提供的介绍文章。另外，如果你对 DDE 的移植感兴趣，请参阅[另一篇侧重于移植相关事项的文章](https://deepin-community.github.io/sig-dde-porting/posts/dde-v23-porting-guide/)。

<!--more-->

不同于 beta3 与 RC 所提供的发布说明，此次将整体介绍 DDE v23 相对 v20 的变化内容。

## 变化较大的默认组件

### `dde-dock` → `dde-shell`

虽然观感上对用户的差异是任务栏整体的变化，但 `dde-shell` 项目所要承载的责任要远超于 `dde-dock`。`dde-shell` 旨在提供一个桌面环境级的外壳程序，使编写 DDE 桌面组件变得更轻松。例如它提供了允许你指定组件的层级关系、确定放置的屏幕位置等功能，并确保相应功能在 x11 与 Wayland 环境的表象均一致。这将使得桌面组件不必针对应用编写重复的代码来实现与桌面环境强相关的功能，并使得后续 x11 向 Wayland 切换变得更方便。当前，`dde-shell` 面向用户呈现的唯一主要组件即任务栏，而预计在后续，OSD、通知中心、剪切板等组件也都会逐渐进行迁移，由 `dde-shell` 统一管理。

另外，`dde-shell` 整体也从传统的 QtWidgets 项目变成了 QML 项目，这使得 GPU 加速可以被有效的利用，使得相应的界面交互、动效等更加流畅。关于“任务栏”这个 `dde-shell` 组件，也存在了较大变化。为了避免插件崩溃时连带整个任务栏组件一起崩溃的问题发生，任务栏区域采用了内嵌 Wayland 合成器的解决方案实现了相关逻辑。

### `dde-launcher` → `dde-launchpad`

`dde-launchpad` 事实上是第一个试水 QML 的 DDE 桌面组件，由于 `dde-launcher` 存在大量的内部 model 状态维护不正确的问题以及界面问题，`dde-launchpad` 则对其进行了整体重构并将整个界面改用 QML 技术进行构建。如 `dde-shell` 一节中所述，这将使得整体的流畅度得以提升。

此外，由于 DDE 计划对应用程序进行相关的权限管控，`dde-launchpad` 也将应用程序列表的获取和管理从原本的 GIO 切换到了新的 `dde-application-manager`。

### `dde-application-manager`

`dde-application-manager` 是一个后台服务，为需要获取应用列表以及启动应用程序的组件（文管、启动器、任务栏等）提供与管理相关数据，为启动的应用与组件设置恰当的 cgroup、环境变量等信息。尽管当下而言 `dde-application-manager` 并无特殊之处，但其为后续实现应用程序权限管控的计划提供了空间。

## 技术预览组件

我们在 v23 的开发过程中引入了技术预览组件的概念，而原本位于技术预览组件的两个主要项目（启动器与 shell）均已离开了技术预览阶段，现以正式版的形式面向社区发布。而目前仍然位于技术预览阶段的项目即 TreeLand 与 deepin-im 了。

为了使得 v23 顺利发布、尽早发布，我们在 RC 阶段即将精力完全投入在了现有组件的缺陷修复上，TreeLand 与 deepin-im 项目均暂无显著成果可供分享，后续我们会在恰当的时候为大家详细介绍这两个项目。

## 后续计划

如之前计划所安排，dde-shell 现已走出技术预览并承载了任务栏的显示职责。dde-shell 的初衷之一是使桌面环境更加模块化，在后续，会有更多桌面组件成为 dde-shell 的一部分。我们也欢迎社区开发者开始尝试使用 dde-shell 编写一些自己觉得有趣的东西，并与我们讨论对 shell 相关设计与接口的体验，一同完善 dde-shell。如果你对这相关的话题感兴趣，欢迎加入 DDE SIG 的 Matrix 群聊 (`#dde:matrix.org`) 之中来。

接下来，wayland 会话支持也会变成主要目标，treeland 将会逐步继续完善，并在恰当的时机提供给大家。

此外，为了方便非 deepin 发行版的用户和开发者使用 DDE，我们也仍然和上次一样提供了一篇移植注意事项博客。笔者也是 DDE 移植 SIG 的成员，对应的文章现已发布到 DDE 移植小组。如果您感兴趣，可[在此阅读](https://deepin-community.github.io/sig-dde-porting/posts/dde-v23-porting-guide/)。如果你本身在参与 DDE 的移植工作，那么也欢迎你加入 DDE 移植小组（`#dde-port:deepin.org` 或 `https://t.me/ddeport`）。

最后，感谢你读到这里。如有任何问题，欢迎在我们的开发者群（`#deepin-community:deepin.org`）进行讨论。
