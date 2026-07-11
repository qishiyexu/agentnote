# Session: 修复 ThorTerminal 多 SSH 标签配置和重复提示符

- 日期：2026-07-11
- Agent：Codex（GPT-5）
- 参与者：Don、Codex
- 话题：修复第二个 SSH 标签找不到会话配置，以及终端提示符重复显示。

## 进展

- 确认配置存储在每个 WebView 中各有一份内存快照；第二次连接复用已有 SSH 窗口时，新会话配置尚未进入该窗口的快照。
- 让临时 `ssh-session:` 配置同步写入同源 `localStorage`，现有 SSH 窗口可在收到新标签事件后立即读取，同时继续保留原有配置快照持久化。
- 第一轮修复了 React StrictMode 下 Tauri 异步事件监听的清理竞态，但用户复测后提示符仍然重复，说明该判断只覆盖了一个真实隐患，并非重复提示符的最终根因。
- 继续检查运行进程的 TCP 连接表，确认一个 SSH 标签实际建立了两条到目标 SSH 端口的连接。StrictMode 会执行一次“挂载、清理、重新挂载”；第一次 `sshOpen` 已进入异步后端，但清理时 Rust 会话表尚未登记完成，`sshClose` 无法关闭它，第二次挂载又创建了另一条真实 SSH shell。
- 将 `sshOpen` 放入零延迟定时任务，并在 effect 清理时取消。StrictMode 的第一次同步清理会取消首次启动，第二次挂载才真正创建连接；生产环境仍只延迟一个事件循环，不引入额外状态层。
- 增加第三个回归检查，覆盖 StrictMode 下不得重复创建 SSH shell。
- 代码提交并推送：`56c6fe5 fix: 修复多 SSH 标签配置和重复提示符`。
- 补充代码提交并推送：`6142a6b fix: 避免 StrictMode 重复创建 SSH 会话`。

## 结论

第二个标签找不到配置来自跨 WebView 配置快照不同步；重复提示符的最终根因是 StrictMode 启动竞态创建了两条真实 SSH shell，不只是重复监听。修复保持现有单 SSH 窗口、多独立标签的设计，不增加新的状态层或依赖。

## 验证

- `pnpm test`：96/96 通过。
- `pnpm build`：通过，仅保留既有 chunk size 提示。
- `git diff --check`：通过。
- 实机连接路由器 `192.168.2.1:2222`：终端只显示一个提示符；ThorTerminal 进程的已建立 SSH TCP 连接由修复前 2 条降为 1 条。

## 待办

- 无。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\lib\configStore.ts`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\SshSessionPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\test\sshSessionWindow.test.ts`
- 新增：`C:\Don\personal\agentnote\sessions\2026-07-11-fix-thor-terminal-multiple-ssh-tabs.md`

## 引用

- 无外部引用。
