# Session: 配置 ThorTerminal Sidecar IPC

- 日期：2026-06-21
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

完成 ThorTerminal 的 1.3 子任务：配置 Tauri Sidecar 机制并验证 IPC 通信。

## 进展

1. 使用 Node.js SEA 将 TypeScript Sidecar 构建成目标 Rust triple 对应的单文件可执行程序，生成文件不提交版本库。
2. 配置 Tauri Shell 插件、`externalBin` 和最小执行权限，由 Rust command 启动 Sidecar 并读取 stdout。
3. 在 React 页面加入 IPC 验证按钮，将 `ping_sidecar` 返回值显示到桌面窗口。
4. 为 Sidecar `--ping` 行为加入 Node 内置测试，并更新 README、架构、状态和变更日志。

## 结论

- 已打通 `React invoke → Tauri Rust command → Node SEA Sidecar → stdout → React UI`。
- 生成的 Windows x64 Sidecar 直接执行 `--ping` 返回 `pong`。
- Tauri debug 桌面程序实际点击验证按钮后显示“Sidecar 回复：pong”。
- 本任务只验证一次性 IPC 链路；`clink.node`、长驻 JSON-RPC 和进程守护仍分别属于 2.1、2.2、2.3。

## 验证

- `pnpm --filter @thor-terminal/sidecar test`
- `pnpm sidecar:build`
- `cargo fmt --check --manifest-path src-tauri/Cargo.toml`
- `cargo clippy --manifest-path src-tauri/Cargo.toml -- -D warnings`
- `pnpm build`
- `pnpm tauri build --debug --no-bundle`
- 启动 `src-tauri/target/debug/thor-terminal.exe` 并在真实桌面窗口验证 `pong`

## 待办

- [ ] 1.4 搭建基础 UI 框架。
- [ ] 2.1 在 Sidecar 中加载 `clink.node` 并验证。
- [ ] 2.2 设计并实现 JSON-RPC 通信协议。
- [ ] 2.3 实现 Rust 端 Sidecar 管理器。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\lib.rs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\tauri.conf.json`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\capabilities\default.json`
- 新增：`D:\Don\Projects\thor_terminal\sidecar-app\scripts\build-sea.mjs`
- 新增：`D:\Don\Projects\thor_terminal\sidecar-app\test\ping.test.mjs`
- 修改：`D:\Don\Projects\thor_terminal\src\App.tsx`
- 修改：`D:\Don\Projects\thor_terminal\README.md`
- 修改：`D:\Don\Projects\thor_terminal\ARCHITECTURE.md`
- 修改：`D:\Don\Projects\thor_terminal\STATUS.md`
- 修改：`D:\Don\Projects\thor_terminal\CHANGELOG.md`

## 引用

- 实现依据 Node.js SEA 与 Tauri Sidecar 官方文档；本次未新增可复用 reference 文档。
