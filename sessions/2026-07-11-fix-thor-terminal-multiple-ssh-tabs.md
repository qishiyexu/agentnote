# Session: 修复 ThorTerminal 多 SSH 标签配置和重复提示符

- 日期：2026-07-11
- Agent：Codex（GPT-5）
- 参与者：Don、Codex
- 话题：修复第二个 SSH 标签找不到会话配置，以及终端提示符重复显示。

## 进展

- 确认配置存储在每个 WebView 中各有一份内存快照；第二次连接复用已有 SSH 窗口时，新会话配置尚未进入该窗口的快照。
- 让临时 `ssh-session:` 配置同步写入同源 `localStorage`，现有 SSH 窗口可在收到新标签事件后立即读取，同时继续保留原有配置快照持久化。
- 确认 React StrictMode 的 effect 清理可能早于 Tauri 异步事件监听注册完成，导致残留两个 `ssh-output` 监听器，同一段 PTY 输出被写入两次。
- 监听注册完成时检查 effect 是否已清理；若已清理便立即注销，避免重复提示符和后续所有输出重复。
- 增加两个回归检查，覆盖跨窗口会话配置同步与异步监听清理竞态。
- 代码提交并推送：`56c6fe5 fix: 修复多 SSH 标签配置和重复提示符`。

## 结论

两个现象来自不同的前端多窗口时序问题，不是 SSH 服务端配置异常。修复保持现有单 SSH 窗口、多独立标签的设计，不增加新的状态层或依赖。

## 验证

- `pnpm test`：95/95 通过。
- `pnpm build`：通过，仅保留既有 chunk size 提示。
- `git diff --check`：通过。

## 待办

- 无。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\lib\configStore.ts`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\SshSessionPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\test\sshSessionWindow.test.ts`
- 新增：`C:\Don\personal\agentnote\sessions\2026-07-11-fix-thor-terminal-multiple-ssh-tabs.md`

## 引用

- 无外部引用。
