# Session: 配置仅对 Codex 生效的本地代理

- 日期：2026-06-20
- Agent：Codex / GPT-5
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

在 Windows 上让 Codex CLI 和 Codex 桌面版默认通过 Clash 本地代理联网，同时不设置用户级或系统级代理环境变量。

## 进展

- 确认 Clash 的 HTTP 代理正在 `127.0.0.1:7890` 监听。
- 确认机器同时安装了 npm 版 Codex CLI 与 Microsoft Store 版 Codex 桌面应用。
- 在 `C:\Users\Don\.local\bin\codex.cmd` 创建 CLI 包装器，只在 Codex 进程树中设置 `HTTP_PROXY`、`HTTPS_PROXY`、`ALL_PROXY` 和 `NO_PROXY`，随后调用原始 CLI。
- 在 `C:\Users\Don\.local\bin\codex-app-proxy.ps1` 创建桌面版启动器，通过 Appx 包位置动态寻找 `Codex.exe`，避免应用升级后固定版本路径失效。
- 在桌面和开始菜单创建 `Codex Proxy` 快捷方式。
- 发现首次创建的快捷方式随后消失，因此重新创建，并在桌面增加不会依赖快捷方式解析的 `Codex Proxy.cmd` 作为兜底入口。
- 验证 CLI 包装器可正常运行 `codex --version`，桌面启动器 PowerShell 语法有效，并确认 Windows 用户级和系统级代理变量仍为空。

## 结论

Codex 已有独立的进程级代理入口，不会改变浏览器、Git 或其它程序的网络设置。当前正在运行的 Codex 桌面进程不会追溯继承新环境；需要完全退出后从 `Codex Proxy` 快捷方式重新启动。

## 待办

- [ ] 若 Clash 代理端口发生变化，同步修改两个启动器中的 `127.0.0.1:7890`。

## 产出

- 新建：`C:\Users\Don\.local\bin\codex.cmd`
- 新建：`C:\Users\Don\.local\bin\codex-app-proxy.ps1`
- 新建：桌面和开始菜单中的 `Codex Proxy` 快捷方式
- 新建：`C:\Users\Don\OneDrive\Desktop\Codex Proxy.cmd`
- 新建：`sessions/2026-06-20-configure-codex-only-proxy.md`
- 修改：`INDEX.md`

## 引用

- 无外部引用。
