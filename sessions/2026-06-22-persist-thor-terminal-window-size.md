# 持久化 ThorTerminal 窗口大小

日期：2026-06-22

## 背景

ThorTerminal 的主窗口和桌面窗口关闭后会回到默认尺寸，用户需要重复调整大小。

## 实现

- 在前端入口监听当前 Tauri 窗口的 resize 事件，将物理像素尺寸写入 WebView localStorage。
- 使用 `window-size:<window label>` 作为键，让主窗口与每个 `desktop-*` 窗口独立保存尺寸。
- 窗口创建时读取已保存尺寸，并通过 Tauri `setSize` 恢复。
- capability 增加 `core:window:allow-set-size`，不引入额外 Rust 插件。

## 验证

- `pnpm build`
- `cargo test --manifest-path src-tauri/Cargo.toml`
- `cargo clippy --manifest-path src-tauri/Cargo.toml -- -D warnings`
- `cargo fmt --manifest-path src-tauri/Cargo.toml -- --check`
- `pnpm tauri build --debug --no-bundle`
- 桌面端实测：窗口从 802×630 调整到 702×630，退出后重新打开仍为 702×630。
