# Session: 补齐 clink Sidecar 自包含运行时

- 日期：2026-06-21
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

将 `clink.node` 所需的第三方 DLL、GStreamer plugins、FFmpeg 和 VC++ runtime 放入 ThorTerminal 的 Windows x64 Sidecar 自包含目录。

## 进展

1. 用 PE 导入表确认 `clink.node` 直接依赖 GLib、GStreamer、FFmpeg、USB、PDFium、QssServiceSDK 和 VC++ runtime。
2. 从 `C:\Don\code\srdcloud\client\clouddesktop-thirdparty-win64` 复制原客户端 Windows x64 运行时到 `D:\Don\Projects\thor_terminal\sidecar-app\lib\win32-x64`。
3. 保持 DLL 与 `clink.node` 同目录，并将 23 个 GStreamer plugins 放在 `lib/gstreamer-1.0`。目录共 156 个文件，约 157.32 MiB。
4. 在不包含源码仓库第三方路径的 `PATH` 中完成验证：23 个 plugins 全部可加载，Node 24.16.0 可加载三个导出并收到 `echo-heart` 的 `type: 201` callback。

## 结论

- Windows x64 Sidecar 运行时目录已自包含，本机加载不再依赖 srdcloud 源码仓库的 `PATH`。
- Sidecar 启动时需将 `win32-x64` 加入 `PATH`，并将 `GST_PLUGIN_PATH` 设为 `win32-x64/lib/gstreamer-1.0`。
- 尚需用项目最终随包 Node 与干净 Windows 机器重复验证。

## 待办

- [ ] 用项目最终随包 Node 20+ runtime 重复 smoke test。
- [ ] 在干净 Windows 机器验证安装包。
- [ ] 实现最小 Sidecar JSON-RPC 适配层并验证真实连接。

## 产出

- 补齐：`D:\Don\Projects\thor_terminal\sidecar-app\lib\win32-x64`
- 修改：`D:\Don\Projects\thor_terminal\docs\RUST_CLINK_INTEGRATION.md`
- 修改：`D:\Don\Projects\thor_terminal\sidecar-app\lib\README.md`
- 修改：`D:\Don\Projects\thor_terminal\STATUS.md`
- 修改：`D:\Don\Projects\thor_terminal\CHANGELOG.md`
- 新建：`sessions/2026-06-21-package-clink-sidecar-runtime.md`
- 修改：`INDEX.md`

## 引用

- [Rust 使用 clink.node 分析](./2026-06-21-analyze-rust-clink-node-integration.md)
- [Windows x64 clink.node 构建](./2026-06-21-build-clink-node.md)
