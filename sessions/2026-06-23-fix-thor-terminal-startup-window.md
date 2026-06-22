# 修复 ThorTerminal 启动后窗口不显示

日期：2026-06-23

## 现象与诊断

- 启动 `src-tauri/target/debug/thor-terminal.exe` 后，进程保持响应并已创建主窗口句柄，但用户看不到窗口。
- Win32 探针确认窗口实际坐标与尺寸在显示器范围内，且不是最小化或屏幕外恢复问题。
- 原启动流程只依赖 Tauri 和 Windows 的默认窗口激活行为，没有显式恢复、显示或聚焦主窗口。

## 修复

- 在 Tauri `setup` 阶段取得 `main` WebView 窗口。
- 依次调用 `unminimize`、`show`、`set_focus`，确保冷启动后主窗口恢复、显示并进入前台。
- 保留现有前端窗口尺寸和位置持久化逻辑。

## 验证

- `cargo fmt --manifest-path src-tauri/Cargo.toml -- --check`
- `cargo test --manifest-path src-tauri/Cargo.toml`：10/10 通过。
- `cargo clippy --manifest-path src-tauri/Cargo.toml -- -D warnings`
- `pnpm test`：3/3 通过。
- `pnpm build`
- `pnpm tauri build --debug --no-bundle`
- 冷启动新构建的 debug 程序后，Win32 探针确认窗口可见、未最小化、进程响应正常，且窗口句柄等于前台窗口句柄。
