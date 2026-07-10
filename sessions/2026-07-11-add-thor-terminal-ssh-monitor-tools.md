# Session: 为 ThorTerminal SSH 窗口增加监控和传输工具面板

- 日期：2026-07-11
- Agent：Codex（GPT-5）
- 参与者：Don、Codex
- 话题：在 SSH 连接窗口底部增加 Snippets、监控、SFTP 传输入口，并统一 SSH 与 SFTP 的标签页工作流。

## 进展

- 将原来固定显示的 Snippets 侧栏改为可切换工具面板，底部工具栏提供 Snippets、监控、SFTP 传输三个图标和文字入口。
- 监控面板复用当前已经认证的 SSH 连接，通过独立 SSH channel 读取 Linux `/proc/stat`、`/proc/meminfo` 和根目录 `df -Pk /`，显示 CPU、内存、磁盘用量。
- 面板显示后立即采样，1 秒后补一次 CPU 差值采样，此后每 5 秒自动刷新；切换或关闭监控面板时停止轮询。
- SFTP 上传和下载在原有调用外层登记活动任务，传输列表显示当前仍在进行中的文件、方向和服务器，底部入口同步显示活动数量。
- 保持既有 SFTP 整文件传输协议，不为尚不存在的分块进度额外引入状态机或依赖。
- 增加中英文界面文案、Rust 指标解析单元测试和前端结构回归检查。
- 代码提交：`fbc66f8 feat: 增加 SSH 监控和传输工具面板`。
- 按反馈将 Snippets 和监控改为独立开关，默认同时显示上方 Snippets 和右下角监控。布局提交：`71ca76e fix: 调整 SSH 右侧工具面板布局`。
- 移除监控标题下方的“每 5 秒自动刷新”提示，保留后台刷新周期。提交：`ad101ca fix: 移除监控刷新提示`。
- 移除监控面板内部标题，底栏“监控”入口文字继续保留。提交：`d295edc fix: 移除监控面板标题`。
- 移除 SSH 配置卡上的 SFTP 按钮；底栏 SFTP 入口改为复制当前 SSH Tab 配置并新建 SFTP Tab，传输列表固定放在文件列表下方。提交：`892a2bb feat: 将 SFTP 入口移至 SSH 会话`。
- 将 SFTP 上传和下载改为 256 KiB 分块传输，Rust 后端按任务 ID 广播已传字节和总字节；每个活动文件在传输列表中显示真实百分比和独立进度条。提交：`163bdea feat: 为 SFTP 文件传输显示真实进度`。
- 下载文件时保持其他文件的下载按钮可用；新任务显示“排队中”，并在当前任务结束后按 FIFO 顺序自动下载。提交：`09cf7de feat: 支持 SFTP 下载排队`。

## 结论

服务器监控只在监控面板可见时工作，并复用已有 SSH 登录态，不会每 5 秒重新登录。磁盘数据当前以服务器根文件系统 `/` 为准；SFTP Tab 在文件列表下方显示活动传输，上传与下载进度均由后端实际完成的分块读写字节驱动，同一 SFTP Tab 的多个下载任务串行排队执行。

## 验证

- `pnpm test`：91/91 通过。
- `pnpm build`：通过，仅保留既有 chunk size 提示。
- `cargo test --manifest-path src-tauri\Cargo.toml`：45/45 通过，仅保留既有 `SidecarManager::restart` 未使用提示。
- `git diff --check`：通过。
- 已推送 `thor_terminal/main`：`09cf7de`。

## 待办

- 无。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\pages\SshSessionPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\SshPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\lib\api.ts`
- 修改：`D:\Don\Projects\thor_terminal\src\i18n.ts`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\ssh.rs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\lib.rs`
- 修改：`D:\Don\Projects\thor_terminal\test\sshSessionWindow.test.ts`
- 新增：`C:\Don\personal\agentnote\sessions\2026-07-11-add-thor-terminal-ssh-monitor-tools.md`
