# Session: 实现 ThorTerminal 桌面连接多窗口流程

- 日期：2026-06-22
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

完成 ThorTerminal 3.4：把已有连接控制链路扩展为桌面会话与独立窗口一一对应的完整连接、聚焦、断开和关窗流程。

## 进展

1. Rust `connect_desktop` 在 Sidecar 建立会话后创建对应的 `desktop-*` Tauri WebView 窗口；同一桌面重复连接时聚焦已有窗口。
2. 新增独立 `/desktop/:desktopId` 页面，展示桌面 ID、会话 ID、连接状态和主动断开按钮。
3. 主动断开会先完成 Sidecar IPC，再关闭对应窗口；用户直接关闭桌面窗口时，Rust 自动断开 Sidecar 会话。
4. 通过 `connection-status-changed` 全局事件同步主窗口和桌面窗口状态，Dashboard 启动后也会批量恢复当前会话状态。
5. 将 `desktop-*` 加入 Tauri capability，并同步更新 API 文档、变更日志和项目状态。

## 结论

- 桌面 ID 使用 UTF-8 字节十六进制生成稳定窗口 label，避免特殊字符造成非法窗口标识或碰撞。
- 桌面路由参数采用百分号编码；窗口 label 和路由编码都由 Rust 单元测试覆盖。
- 当前独立窗口已经完成会话生命周期和多窗口管理；远程画面区域保留为 clink 渲染接入点，本任务不提前实现渲染协议。
- 桌面程序已真实启动并确认主界面无布局回归；由于验证环境未登录管理台，未使用真实账号桌面执行连接点击，窗口生命周期由构建、单元测试和代码路径验证覆盖。

## 验证

- `pnpm build`
- `cargo test --manifest-path src-tauri/Cargo.toml`
- `cargo clippy --manifest-path src-tauri/Cargo.toml -- -D warnings`
- `pnpm --filter @thor-terminal/sidecar test`
- `cargo fmt --check`
- `git diff --check`
- `pnpm tauri build --debug --no-bundle`
- 启动 `src-tauri\target\debug\thor-terminal.exe`，检查未登录主界面并正常退出

## 待办

- [ ] 接入 clink 远程画面渲染与输入事件。
- [ ] 在具备管理台登录态的环境补充真实多桌面并发连接验收。

## 产出

- 新增：`D:\Don\Projects\thor_terminal\src\pages\DesktopWindowPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\lib.rs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\capabilities\default.json`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\DashboardPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\router.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\i18n.ts`
- 修改：`D:\Don\Projects\thor_terminal\docs\API_SPEC.md`
- 修改：`D:\Don\Projects\thor_terminal\CHANGELOG.md`
- 修改：`D:\Don\Projects\thor_terminal\STATUS.md`

## 引用

- 本次沿用 ThorTerminal 既有 Sidecar JSON-RPC 与 UI 架构，未新增外部引用。
