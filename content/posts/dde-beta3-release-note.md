---
title: "DDE v23 beta3 发布说明"
date: 2022-09-16T14:00:00+08:00
draft: false
authors: ["BLumia"]
tags: ["成果公示"]
---

deepin v23 beta3 将于 2024 年 1 月 31 日发布（注：因存在部分临时设计变动，延期发布，仍计划在当周发布），这里为大家简要描述本次更新中，DDE 所涉及的变更，以及我们的进一步计划。另需注意，本文章倾向于对 DDE 项目整体的技术内容进行描述，面向 DDE 开发者和对 DDE 开发感兴趣的读者，并非面向最终用户的特性概览文章。

<!--more-->

## 变化较大的默认组件

相比 deepin v23 beta2 而言，在于 deepin v23 beta3 发布的 DDE 中，有两个项目得到了较大规模的重构，并随 beta3 默认提供给各位用户。这两个项目分别是：

- dde-application-manager (>= 1.2)
- dde-launchpad (取代 dde-launcher)

dde-application-manager 承载了 DDE 下应用程序的启动与管理等职责，在早期版本中存在诸多架构不合理以及实现问题，影响到了 dde-launcher、dde-dock、dde-grand-search 等项目启动应用与管理应用功能的正常使用。于是我们对 dde-application-manager 进行了大规模的重构，也重新设计了此项目的对外 D-Bus 接口。在 beta3 中，你将不再会遇到类如应用频繁集体闪退的问题，对应用 desktop 文件的支持变得更加完整。在未来，也会对实时调整应用缩放等功能预留支持的空间。

dde-launchpad 则是对 dde-launcher 的完整重写。一方面，dde-launcher 对旧的 dde-application-manager 具有很高的偶合度，另一方面，旧的 dde-launcher 架构使得维护此组件变得非常困难。dde-launchpad 是目前 DDE 的首个基于 QML 的组件，且基于最新的 Qt 6。得益于 QML 技术，launchpad 将会有效利用 GPU 绘制界面，使交互体验更加流畅。不过由于 dde-launchpad 是完全重写版本，故尚有部分功能未在当前的 dde-launchpad 中提供。

## 技术预览组件

*非技术用户请慎重启用技术预览功能*

deepin v23 beta2 时，我们提供了一个需要用户手动安装的 dcc-insider-plugin 插件，称为技术预览插件。这个组件旨在帮助用户方便的测试 deepin 未来版本中计划提供但仍不稳定的系统组件。在 beta2 时，技术预览组件仅包含了 dde-launchpad 一项。而在 beta3 中，dde-launchpad 走出了技术预览阶段，而有更多的组件进入了技术预览阶段：

- treeland
- dde-shell
- deepin-im

treeland 是 deepin 的下一代 wayland 窗口合成器，且可以同时提供会话管理功能。由于 treeland 会替换 deepin-kwin 并同样会接管 lightdm，故切换 treeland 时需要格外留意。

切换到 treeland 后，将会进入的 DDE 会话也会与原本有差异。treeland 会话中会使用 dde-shell 而非原本的 dde-dock、dde-widgets 等组件。尽管 dde-shell 可以独立使用，但目前 dde-shell 是作为与 treeland 共同配合进入技术预览阶段的桌面组件。dde-shell 提供了与 wayland 会话下的桌面环境的相关功能集成，今后的 dock 等组件也均会作为 dde-shell 的插件存在，以便提供更深度的集成。

deepin-im 是新的输入法组件，作为目前各种底层输入法框架的抽象层存在，以便用户更方便的管理输入法而无需关注底层的配置细节。deepin-im 并不与 treeland 绑定，可以单独启用。

## 后续计划

在未来，首要计划即使 dde-shell 可以走出技术预览阶段并配合 treeland 达成真正稳定可用的 wayland 会话，这意味着 treeland 与 dde-shell 相关的项目还有比较多的相关工作需要持续进行。也是我们的主要努力方向。另外，尽管 dde-shell 相关的 API 可能仍会存在一些变动，但我们计划在随后提供一些必要的文档来帮助开发者更好的了解 dde-shell 的目的与作用，以便帮助开发者参与到项目之中来。如果你对这相关的话题感兴趣，欢迎加入 DDE SIG 的 Matrix 群聊 (`#dde:matrix.org`) 之中来。

另外，由于 DDE 在 beta2 与 beta3 的变化较大，我们也计划提供一篇移植注意事项博客。笔者也是 DDE 移植 SIG 的成员，于是对应的文章计划会发布到 DDE 移植小组。如果您感兴趣，请考虑订阅 <planet.deepin.org> 的 RSS 更新。如果你本身在参与 DDE 的移植工作，那么也欢迎你加入 DDE 移植小组（`#dde-port:deepin.org` 或 `https://t.me/ddeport`）。

最后，感谢你读到这里。如有任何问题，欢迎在我们的开发者群（`#deepin-community:deepin.org`）进行讨论。
