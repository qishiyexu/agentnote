# Session: 为 ThorTerminal SSH 窗口增加监控和传输工具面板

- 日期：2026-07-11
- Agent：Codex（GPT-5）
- 参与者：Don、Codex
- 话题：在 SSH 连接窗口底部增加 Snippets、监控、SFTP 传输入口，并由按钮控制右侧面板显示。

## 进展

- 将原来固定显示的 Snippets 侧栏改为可切换工具面板，底部工具栏提供 Snippets、监控、SFTP 传输三个图标和文字入口；再次点击当前入口可收起右侧面板。
- 监控面板复用当前已经认证的 SSH 连接，通过独立 SSH channel 读取 Linux `/proc/stat`、`/proc/meminfo` 和根目录 `df -Pk /`，显示 CPU、内存、磁盘用量。
- 面板显示后立即采样，1 秒后补一次 CPU 差值采样，此后每 5 秒自动刷新；切换或关闭监控面板时停止轮询。
- SFTP 上传和下载在原有调用外层登记活动任务，传输面板显示当前仍在进行中的文件、方向和服务器，底部入口同步显示活动数量。
- 保持既有 SFTP 整文件传输协议，不为尚不存在的分块进度额外引入状态机或依赖。
- 增加中英文界面文案、Rust 指标解析单元测试和前端结构回归检查。
- 代码提交：`fbc66f8 feat: 增加 SSH 监控和传输工具面板`。
- 按反馈将三个工具改为独立开关，默认同时显示上方 Snippets 和右下角监控；SFTP 面板开启时插在二者之间。布局提交：`71ca76e fix: 调整 SSH 右侧工具面板布局`。
- 移除监控标题下方的“每 5 秒自动刷新”提示，保留后台刷新周期。提交：`ad101ca fix: 移除监控刷新提示`。
- 移除监控面板内部标题，底栏“监控”入口文字继续保留。提交：`d295edc fix: 移除监控面板标题`。

## 结论

服务器监控只在监控面板可见时工作，并复用已有 SSH 登录态，不会每 5 秒重新登录。磁盘数据当前以服务器根文件系统 `/` 为准；SFTP 面板按需求显示活动文件列表，不伪造无法从现有整文件 API 获得的百分比进度。

## 验证

- `pnpm test`：89/89 通过。
- `pnpm build`：通过，仅保留既有 chunk size 提示。
- `cargo test --manifest-path src-tauri\Cargo.toml`：45/45 通过，仅保留既有 `SidecarManager::restart` 未使用提示。
- `git diff --check`：通过。
- 已推送 `thor_terminal/main`：`d295edc`。

## 待办

- 无。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\pages\SshSessionPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\lib\api.ts`
- 修改：`D:\Don\Projects\thor_terminal\src\i18n.ts`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\ssh.rs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\lib.rs`
- 修改：`D:\Don\Projects\thor_terminal\test\sshSessionWindow.test.ts`
- 新增：`C:\Don\personal\agentnote\sessions\2026-07-11-add-thor-terminal-ssh-monitor-tools.md`
