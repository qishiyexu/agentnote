# Session: 分析 Rust 使用 clink.node

- 日期：2026-06-21
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

分析 ThorTerminal 中新生成的 `sidecar-app/lib/win32-x64/clink.node`，明确 Rust 应如何使用，并把二进制、源码和实际加载验证结果写入项目文档。本记录替代本会话早先针对 `libclink.lib` 的分析。

## 进展

1. 确认目标是 12,290,560 字节的 x64 Release Node-API 扩展，SHA-256 为 `6164C980F55BB4D102E3A727170534B68D408EA9415978C7CD4373DDECAE25DE`，与 `deploy/clink/Windows/win64` 下的 output、pack、sym 产物一致。
2. 核对 `clinkProxy.cc` 和构建文件，确认模块导出 `_ClinkCallee`、`_ClinkGetImage`、`_ClinkSendData`，并依赖 Node-API、GLib/GStreamer main loop 和 thread-safe callback。
3. 通过 PE 依赖与第三方目录核对运行时闭包；仅有 `clink.node` 不能独立发行，还需要 GLib、GStreamer、FFmpeg、USB、PDFium、VC++ runtime 和 GStreamer plugins。
4. 在 Node 24.16.0 / N-API 10 中完成实际 `require()` 和 `echo-heart` callback smoke test，收到 `type: 201`、`value: 0`。
5. 重写 ThorTerminal 的集成文档、库目录说明、状态和变更日志，明确 Rust 不直接加载 `.node`，而是通过 Node Sidecar 的逐行 JSON-RPC 使用它。

## 结论

- `clink.node` 不是可供 Rust `extern "C"` 或 `libloading` 直接使用的普通业务 DLL；它需要 `node.exe` 提供 Node-API 上下文。
- 正确架构是 `React → Tauri/Rust → JSON-RPC stdio → Node Sidecar → clink.node → 云电脑`，与项目既定约束一致。
- Sidecar 必须在第一次 `_ClinkCallee` 前设置 `global._ClinkCaller`，并负责 clink 内部 id/context、Buffer 边界和 callback 到外层 JSON-RPC 的转换。
- `run` 返回 context 只表示请求已受理，不代表连接成功；context 是进程内不透明 token，不能持久化或跨 Sidecar 复用。
- 当前 `.node` 已验证能加载和回调，但尚未完成真实云电脑连接，也未形成自包含运行时目录。

## 待办

- [ ] 把第三方 DLL、GStreamer plugins 和 VC++ runtime 整理为 Sidecar 自包含目录。
- [ ] 用项目最终随包 Node 20+ 的精确版本重复加载与 callback 测试。
- [ ] 实现最小 Sidecar JSON-RPC 适配层，验证真实 `run → context → 连接状态 → stop`。
- [ ] 在干净 Windows 机器验证安装包、重复连接、异常退出和多 session。

## 产出

- 重写：`D:\Don\Projects\thor_terminal\docs\RUST_CLINK_INTEGRATION.md`
- 修改：`D:\Don\Projects\thor_terminal\sidecar-app\lib\README.md`
- 修改：`D:\Don\Projects\thor_terminal\README.md`
- 修改：`D:\Don\Projects\thor_terminal\STATUS.md`
- 修改：`D:\Don\Projects\thor_terminal\CHANGELOG.md`

## 引用

- ThorTerminal 主文档：`D:\Don\Projects\thor_terminal\docs\RUST_CLINK_INTEGRATION.md`
