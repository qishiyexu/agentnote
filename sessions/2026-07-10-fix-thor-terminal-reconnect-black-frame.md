# Session: 修复 ThorTerminal 多次连接后变慢和画面黑块

- 日期：2026-07-10
- Agent：Codex
- 参与者：Don、Codex

## 现象

同一云桌面短时间内多次连接、断开后，下一次连接明显变慢；连接成功后，原生直绘窗口的部分区域保持黑色。

## 根因

- Sidecar 的 `disconnect` 只发出 clink `stop` 就立即返回，`connect` 发现旧会话时也不等待其 `connect-state=5` 清理完成。日志中同一桌面的 reconnect context map 已累积到 3 个，说明新旧原生上下文发生了重叠。
- 旧会话清理未完成时，桌面级 `nativeRenderers` 标记仍存在；新会话会把原生渲染器误判为非首次挂载，跳过 `invalidate-full-image`。
- 原生渲染器收到第一块局部脏矩形就显示窗口。新分配的整帧画布以零填充，未被该局部帧覆盖的区域因此显示为黑色。

## 修复

- clink stop 生命周期增加 Promise waiter；单桌面重连、普通断开和全部断开都等待旧 context 收到 `connect-state=5` 并完成缓存、屏幕状态、渲染器状态清理后再继续，保留 10 秒兜底。
- 发起 stop 时立即撤销桌面级原生渲染器标记，避免重连继承旧会话的首次挂载状态。
- Windows 原生渲染器记录 `has_full_frame`：画布尺寸变化后先隐藏子窗口，局部帧可以合并但不会显示，直到收到覆盖完整画布的帧后才显示；之后继续正常应用局部脏矩形。

## 验证

- 先新增回归测试并确认旧实现失败：连接清理等待、完整首帧显示门控各失败 1 项。
- `pnpm test`：70/70 通过。
- `sidecar-app` 下 `pnpm test`：24/24 通过。
- `pnpm build`：通过。
- `pnpm sidecar:build`：通过，真实编译 `clink-direct.cc` 并重新生成 Windows SEA sidecar。
- 新生成的 `thor-sidecar-x86_64-pc-windows-msvc.exe` 实际 ping 返回 `pong`。
- `src-tauri` 下 `cargo test`：40/40 通过。
- `git diff --check`：通过。

## 验收边界

当前自动验证覆盖连接生命周期、首帧门控、原生 C++ 编译、SEA 打包和 RPC 启动；最终仍应在有真实云桌面的环境中连续连接/断开多轮，目视确认连接耗时不再累积且画面没有黑块。

## 后续回归：第二次连接卡在 session 创建中

首轮修复后，真实环境暴露出更直接的二次连接问题：第一次断开后，clink 很快复用了相同的 context 指针；Sidecar 仍将该指针保留在 `closedContexts` 60 秒，并在消息入口丢弃该 context 的全部回调。因此原生侧第二次连接实际上已依次进入 Connecting、Connected、FirstImage，代理层却始终停留在“返回状态：session 已创建”。

修复是在新 session 写入 `sessions` 和 `desktopByContext` 前调用 `closedContexts.delete(context)`，明确让被原生复用的 context 重新生效。增加了回归断言，确认新 session 注册顺序不会再次漏掉该状态转换。代码提交：`d42e793 fix: 允许重连复用 clink context`。

自动验证结果仍为：根目录 `pnpm test` 70/70、Sidecar `pnpm test` 24/24、Rust `cargo test` 40/40，`pnpm build` 与 `pnpm sidecar:build` 均通过。真实第二次连接由 Don 手动验收。
