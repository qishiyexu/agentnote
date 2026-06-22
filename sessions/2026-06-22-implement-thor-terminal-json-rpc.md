# Session: 实现 ThorTerminal JSON-RPC 通信协议

- 日期：2026-06-22
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

完成 ThorTerminal 2.2：设计并实现 Rust 与 Node Sidecar 之间的 JSON-RPC 通信协议。

## 进展

1. Sidecar 新增长驻 stdin/stdout 逐行 JSON-RPC 2.0 服务，保留原有诊断子命令。
2. 实现 `ping`、通知不响应、包络校验，以及 `-32700/-32600/-32601` 标准错误。
3. Rust 侧新增请求、响应、ID 和错误的数据模型，为 2.3 Sidecar 管理器复用。
4. 新增 Node 进程级协议测试和 Rust 序列化测试，并同步 API 规范、状态和变更日志。

## 结论

- 2.2 只实现协议边界；进程守护、pending 请求、超时和事件转发归 2.3。
- `connect` 等 clink 业务方法归 2.4，当前不提供假实现。
- stdout 仅输出一行一个 JSON-RPC 对象，日志继续写 stderr。

## 验证

- `pnpm --filter @thor-terminal/sidecar test`
- `cargo test --manifest-path src-tauri/Cargo.toml`
- `cargo fmt --check --manifest-path src-tauri/Cargo.toml`
- `cargo clippy --manifest-path src-tauri/Cargo.toml -- -D warnings`
- `pnpm build`
- `pnpm sidecar:build`，并向生成的 SEA 可执行文件 stdin 写入 `ping` JSON-RPC 请求验证 `pong` 响应
- `git diff --check`

## 待办

- [ ] 2.3 实现 Rust 端 Sidecar 管理器（含进程守护、pending、超时和事件转发）。
- [ ] 2.4 实现前端连接控制指令和 Sidecar 业务方法。

## 产出

- 新增：`D:\Don\Projects\thor_terminal\sidecar-app\src\rpc.ts`
- 新增：`D:\Don\Projects\thor_terminal\sidecar-app\test\rpc.test.mjs`
- 新增：`D:\Don\Projects\thor_terminal\src-tauri\src\protocol.rs`
- 更新：`D:\Don\Projects\thor_terminal\docs\API_SPEC.md`
- 更新：`D:\Don\Projects\thor_terminal\STATUS.md`
- 更新：`D:\Don\Projects\thor_terminal\CHANGELOG.md`

## 引用

- [ThorTerminal API 规范](../../../Projects/thor_terminal/docs/API_SPEC.md)
