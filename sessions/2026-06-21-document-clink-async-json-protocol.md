# Session: 整理 clink 内部异步 JSON 协议

- 日期：2026-06-21
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

基于 clink 当前源码整理 Node Sidecar 与 `clink.node` 之间的内部异步 JSON 协议，并形成 ThorTerminal 可直接用于实现 Sidecar 的独立文档。

## 进展

1. 核对 `clink-websockets.c/.h`、`clink-def.h`、`clinkProxy.cc`、`context_utils.cpp` 和 `clink.c`，确认消息类型、GLib 异步投递、Node thread-safe callback 与连接状态来源。
2. 整理 `100/101/201/202` 包络、context 生命周期、`run → connect-state → stop/reconnect` 时序，以及常用命令和关键事件。
3. 从当前源码静态提取完整的输入命令与输出事件名称清单。
4. 明确原生入口的兼容性边界：非法 JSON、未知类型和缺 context 可能静默无回调；未知命令可能返回空成功；连接成功必须以事件而不是 `run` response 判断。
5. 将新文档接入 ThorTerminal README、Rust 集成分析、状态和变更日志。

## 结论

- `_ClinkCallee()` 的同步返回值不是业务结果，所有业务结果和事件都经 `global._ClinkCaller` 异步返回。
- `run` 的 `201 + data.context` 只表示 context 已创建，真正连接成功是 `connect-state` 的 `state = 2`；首帧到达是 `state = 3`。
- context 是当前 Sidecar 进程内的不透明指针 token，不能解析、持久化或跨进程复用。
- 当前通用命令分发基本只发送 `201`，Sidecar 必须承担输入校验、pending id、超时、事件状态机和错误转换。
- ThorTerminal 应继续保持两层协议：Rust ↔ Sidecar 使用 JSON-RPC，Sidecar ↔ clink 使用本文记录的内部 JSON。

## 待办

- [ ] 按协议文档实现最小 Sidecar adapter 和 pending/context 映射。
- [ ] 用真实但可控的连接配置验证 `run → connect-state → stop`。
- [ ] 根据 ThorTerminal 实际功能逐步补充所用命令的精确 `arg` schema，不提前封装全部 clink 能力。

## 产出

- 新建：`D:\Don\Projects\thor_terminal\docs\CLINK_ASYNC_JSON_PROTOCOL.md`
- 修改：`D:\Don\Projects\thor_terminal\docs\RUST_CLINK_INTEGRATION.md`
- 修改：`D:\Don\Projects\thor_terminal\README.md`
- 修改：`D:\Don\Projects\thor_terminal\STATUS.md`
- 修改：`D:\Don\Projects\thor_terminal\CHANGELOG.md`

## 引用

- ThorTerminal 协议文档：`D:\Don\Projects\thor_terminal\docs\CLINK_ASYNC_JSON_PROTOCOL.md`
- 本次结论来自本地 clink 源码，无外部引用。
