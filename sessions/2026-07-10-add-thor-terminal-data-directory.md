# Session: 为 ThorTerminal 增加独立数据目录

- 日期：2026-07-10
- Agent：Codex（GPT-5）
- 参与者：Don、Codex
- 话题：保留原配置目录，新增独立数据目录，并统一日志类数据的默认落盘位置。

## 进展

- 保留原有“配置目录”设置和 `set_config_dir` 切换流程，不把配置目录与数据目录合并。
- 在设置页增加与“日志接口密钥”平级的“数据目录”面板；新增独立 `set_data_dir` 命令，目录值保存在全局配置的 `dataDir` 字段中。
- 未设置数据目录时，默认使用 `%LOCALAPPDATA%\com.thorterminal.desktop`；设置后会自动创建目标目录。
- 移除可单独配置的“日志下载目录”，并清理旧 `logDownloadDir` 配置；取日志和 xlog 解密固定使用 `<数据目录>\logdownload`。
- 服务器日志“批量收集”不再弹出目录选择框，固定使用 `<数据目录>\server_log_YYYY-MM-DD`；单项“收集”仍保留目录选择框。
- Tauri 应用日志和 sidecar 调试日志改为写入 `<数据目录>\logs`。切换数据目录后，下载立即使用新目录；已经启动的日志组件在应用重启后完全切换。
- 日志导出和删除继续兼容旧默认目录中的历史日志。
- 代码提交：`3e8d00a feat: 新增独立数据目录`。

## 结论

配置目录与数据目录是两套独立状态：配置文件、SSH 配置和机器级设置继续遵循原配置目录；下载、服务器批量日志和运行日志遵循数据目录。数据目录是配置项，不会反向修改配置目录。

## 验证

- `pnpm test`：84/84 通过。
- `pnpm build`：通过，仅保留既有 chunk size 提示。
- `cargo test`：42/42 通过。
- `pnpm --filter @thor-terminal/sidecar test`：24/24 通过。
- `git diff --check`：通过。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\pages\SettingsPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\LogDownloadPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\ServerLogCollectionPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\config.rs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\logs.rs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\sidecar.rs`
- 修改：`D:\Don\Projects\thor_terminal\sidecar-app\src\rpc.ts`
- 新增：`D:\Don\Projects\thor_terminal\src\lib\dataPaths.ts`
- 新增：`D:\Don\Projects\thor_terminal\test\dataDirectoryIntegration.test.ts`
- 新增：`D:\Don\Projects\thor_terminal\test\dataPaths.test.ts`

## 引用

- 无外部引用。
