# 持久化 ThorTerminal 窗口大小和位置

日期：2026-06-22

## 背景

ThorTerminal 的主窗口和桌面窗口关闭后会回到默认尺寸和默认位置，用户需要重复调整。

## 实现

- 在前端入口监听当前 Tauri 窗口的 resize / move 事件，将物理像素尺寸和屏幕位置写入 WebView localStorage。
- 使用 `window-size:<window label>` 作为键，让主窗口与每个 `desktop-*` 窗口独立保存窗口边界；旧的仅尺寸数据仍可读取。
- 窗口创建时读取已保存边界，并通过 Tauri `setSize` / `setPosition` 恢复。
- capability 增加 `core:window:allow-set-size` 和 `core:window:allow-set-position`，不引入额外 Rust 插件。

## 验证

- `pnpm build`
- `cargo test --manifest-path src-tauri/Cargo.toml`
- `cargo clippy --manifest-path src-tauri/Cargo.toml -- -D warnings`
- `cargo fmt --manifest-path src-tauri/Cargo.toml -- --check`
- `pnpm tauri build --debug --no-bundle`
- 桌面端实测：窗口移动到 607,684 且保持 702×630，退出后重新打开仍恢复到 607,684 / 702×630。
