# Session: 分析 Rust 集成 libclink 静态库

- 日期：2026-06-21
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

核对 ThorTerminal 中 `libclink.lib` 的来源、ABI、依赖和运行模型，判断 Rust 应如何使用，并将结论写入项目文档。

## 进展

1. 确认 `sidecar-app/lib/win32-x64` 中三个静态库与 clink 生成工程 `output/Debug` 的产物哈希完全一致。
2. 通过 `dumpbin` 确认核心入口是可供 Rust `extern "C"` 调用的 C 符号，同时确认库要求 VS2015 Debug CRT、Debug C++ ABI 和 `_ITERATOR_DEBUG_LEVEL=2`。
3. 阅读 `clink-api.h`、`clink-websockets.c`、`context_utils.cpp`、`clink-display.c` 和现有 `clinkProxy.cc`，还原异步 JSON callback、context token、图像缓冲区、GStreamer 与 Win32 message window 的约束。
4. 从生成的 `clinkProxy.vcxproj` 核对完整链接闭包，确认当前三个 `.lib` 缺少 GLib、GStreamer、FFmpeg、OpenSSL、设备模块及运行时资源，不能直接组成 Rust 发行包。
5. 在 ThorTerminal 仓库写入完整分析、库目录说明、进度记录和变更日志，没有把现有 Phase 任务误标为完成。

## 结论

- Rust 技术上能调用 clink 的 C ABI，但当前三件套只是 Debug 静态产物，不能直接链接并交付。
- ThorTerminal 当前应继续采用既定的 Node.js Sidecar + `clink.node` 架构。
- 如果以后决定移除 Node，优先在 clink 原 CMake 工程中构建一个稳定的 Release C ABI bridge DLL，由它管理第三方链接、GStreamer 初始化、callback 和内存边界；Cargo 直接静态链接只适合 POC。
- `clink_js_api_call()` 是异步投递，返回值通常为 `NULL`；`run` 的成功响应只代表 context 已创建，不代表云电脑已连接。

## 待办

- [ ] 在 clink 构建系统中生成完整 x64 Release `clink.node` 和运行时目录，并做独立加载/连接 smoke test。
- [ ] 若产品决定改为 Rust 原生集成，先建立架构决策，再实现并验证 `clink-ffi.dll`。

## 产出

- 新建：`D:\Don\Projects\thor_terminal\docs\RUST_CLINK_INTEGRATION.md`
- 修改：`D:\Don\Projects\thor_terminal\sidecar-app\lib\README.md`
- 修改：`D:\Don\Projects\thor_terminal\README.md`
- 修改：`D:\Don\Projects\thor_terminal\STATUS.md`
- 修改：`D:\Don\Projects\thor_terminal\CHANGELOG.md`

## 引用

- ThorTerminal 主文档：`D:\Don\Projects\thor_terminal\docs\RUST_CLINK_INTEGRATION.md`

