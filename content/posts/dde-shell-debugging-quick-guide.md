---
title: "dde-shell 调试简明指南"
date: 2026-01-19T17:20:00+08:00
draft: false
authors: ["BLumia"]
tags: ["教程"]
---

桌面环境组件的开发调试相比应用组件总会有或多或少的不同，本文就旨在提供一个快速简明的描述，给期望开发或调试 dde-shell 以及其插件的开发者一个参考，以便快速上手。

## 环境推荐

你实际上可以使用任意自己喜欢的 IDE/开发环境，方便起见，此处以下列环境作为基础。若你使用其他开发工具或环境，则需自己参照实际情况进行调整。

- IDE: Visual Studio Code 或其衍生版本
  - 推荐安装的插件
    - [Native Debug (webfreak.debug)](https://marketplace.visualstudio.com/items?itemName=webfreak.debug) (用于使用 gdb 启动调试)
    - [clangd (llvm-vs-code-extensions.vscode-clangd)](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd) (用于提供 C++ LSP)
    - [CMake Tools (ms-vscode.cmake-tools)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools) (用于初始的配置和构建)

推荐使用 VSCode 或衍生版本是因为 VSCode 目前事实上是主流 IDE。后附的三个插件均是 VSCode 官方仓库以及 [Open VSX 仓库](https://open-vsx.org/) 都可以获取到的。下面所涉及的配置文件主要会和 Native Debug 插件有一定相关性。

## 构建项目

事实上，dde-shell 的项目和常规 CMake 项目并无不同，常规的 CMake 配置和构建过程即适用于 dde-shell 项目。不过为了更好的 IDE 集成，建议确保 `-DCMAKE_EXPORT_COMPILE_COMMANDS=ON` 配置以便在构建目录中生成可供 clangd 消费的 `compile_commands.json`。

你亦可直接使用 CMake Tools 完成配置和构建，也是本文推荐的做法。好处是你还可以在 VSCode 中看到相关的单元测试，以及直接在 IDE 中点击相关按钮完成整个构建过程。

> [!NOTE]
> 请注意：不要进行安装（`make install` / `cmake --build build --target install`），因为默认的安装位置是 `/usr/local/bin`，并非期望的位置。盲目手动安装意味着卸载也会非常困难。若要运行自己所构建的版本，请参阅后面的部分。

## 运行项目

dde-shell 项目在 DDE 桌面环境承载的责任包括使对应组件能够准确的在屏幕的指定位置进行展示。dde-shell 作为一个桌面环境外壳程序，自身也是由多个不同的插件构成的。dde-shell 需要通过参数来获知自己需要加载或排除加载哪些插件。对于任务栏组件而言，任务栏的部分插件也涉及到了一些额外的环境变量需要关心。

下面是一个适用于 VSCode + Native Debug 的构建和运行模板(`.vscode/launch.json`)：

```jsonc
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug",
            "type": "gdb",
            "request": "launch",
            "target": "./build/shell/dde-shell",
            "cwd": "${workspaceRoot}",
            "valuesFormatting": "prettyPrinters",
            "env": {
                "TRAY_LOADER_EXECUTE_PATH": "/usr/libexec/trayplugin-loader",
                "TRAY_DEBUG_PLUGIN_PATH": "/usr/lib/dde-dock/plugins/:/usr/lib/dde-dock/plugins/system-tray/",
                "DISPLAY": ":0",
                "QT_QPA_PLATFORM": "dxcb",
                "LANGUAGE": "zh_CN.UTF-8",
                "QT_LOGGING_RULES": "*.debug=true;qt*.debug=false",
                "DDE_SHELL_PLUGIN_PATH": "/home/username/Sources/dde-launchpad/build/plugins/",
                "DDE_SHELL_PACKAGE_PATH": "/home/username/Sources/dde-launchpad/build/packages/",
                // "DDE_SHELL_PLUGIN_PATH": "/usr/lib/x86_64-linux-gnu/dde-shell/",
                // "DDE_SHELL_PACKAGE_PATH": "/usr/share/dde-shell/",
                "XDG_CURRENT_DESKTOP": "DDE",
            },
            "arguments": "-p org.deepin.ds.dock -p org.deepin.ds.dde-apps",
            "preLaunchTask": "Build (Debug)"
        },
    ]
}
```

以及对应的 `.vscode/tasks.json`

```jsonc
{
  "version": "2.0.0",
  "tasks": [
    {
        "label": "Build (Debug)",
        "type": "shell",
        "command": "cmake",
        "args": ["--build", "build", "-j4"]
    },
    {
        "label": "Run (x11)",
        "type": "shell",
        "command": "./build/shell/dde-shell",
        "args": [
          "-p", "org.deepin.ds.dock",
          "-p", "org.deepin.ds.dde-apps",
          "-p", "org.deepin.ds.example.bridge"
        ],
        "options": {
          "env": {
            "TRAY_LOADER_EXECUTE_PATH": "/usr/libexec/trayplugin-loader",
            "TRAY_DEBUG_PLUGIN_PATH": "/usr/lib/dde-dock/plugins/:/usr/lib/dde-dock/plugins/system-tray/",
            "DISPLAY": ":0",
            "QT_QPA_PLATFORM": "dxcb",
            "LANGUAGE": "zh_CN.UTF-8",
            "QT_LOGGING_RULES": "*.debug=true;qt*.debug=false",
            "DDE_SHELL_PLUGIN_PATH": "/home/username/Sources/dde-launchpad/build/plugins/",
            "DDE_SHELL_PACKAGE_PATH": "/home/username/Sources/dde-launchpad/build/packages/",
            "XDG_CURRENT_DESKTOP": "DDE",
          }
        }
    }
  ]
}
```

若只是从 VSCode 中运行，则事实上 `.vscode/tasks.json` 中的 `Run (x11)` 部分是不必要的。后者是供 [vscode-task-runner](https://github.com/NathanVaughn/vscode-task-runner) 类工具调用使用的，这对目前的各类 Agent 助手类应用会更有帮助。

上述列出的环境变量和参数列表均为常用内容，对于各类 bug 很可能需要做出相应调整。例如：

- 加载自己构建的插件还是系统的插件？是否要加载其他外部插件（比如 dde-launchpad 插件？）（`DDE_SHELL_PLUGIN_PATH`/`DDE_SHELL_PACKAGE_PATH`）
- 测试其他语言的效果？（`LANGUAGE`）
- 使用自己构建的 tray-loader 或者加载自己构建的托盘插件？（`TRAY_LOADER_EXECUTE_PATH`/`TRAY_DEBUG_PLUGIN_PATH`）
- 确保日志输出？

另外，如果你要测试 TreeLand 环境下的 dde-shell，你需要移除 `DISPLAY` 环境变量并添加下面的环境变量（推荐另写一组或多组 launch configuration）：

```jsonc
// 窗口化 treeland
               "WAYLAND_DISPLAY": "wayland-0",
                "QT_QPA_PLATFORM": "wayland",
// 常规 treeland 会话
                "WAYLAND_DISPLAY": "treeland.socket",
                "QT_QPA_PLATFORM": "wayland",
```

## 结语

希望上述快速指南对你有帮助。若有问题，欢迎在 GitHub linuxdeepin/developer-center 仓库发起讨论。你也可以考虑添加相关实时群聊来与开发者进行交流（ [Telegram](http://t.me/deepin_community) / [Matrix](https://matrix.to/#/%23deepin-community:matrix.org) ）。
