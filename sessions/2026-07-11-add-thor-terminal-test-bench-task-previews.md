# Session: 为 ThorTerminal 测试台增加重连任务、画面和资源曲线

- 日期：2026-07-11
- Agent：Codex（GPT-5）
- 参与者：Don + Codex

## 话题

在测试台为每台云桌面增加连接持续时间，并在页面下方展示正在运行的测试任务、状态、定时刷新的虚机画面缩略图，以及承载测试任务的 Sidecar 进程资源曲线。

## 进展

- 复用已有 `test_bench_connect`、`test_bench_disconnect` 和 `get_desktop_frame`，没有增加新的 Rust 接口。
- 连接持续时间使用毫秒；留空时只连接一次并保持连接，填写后按“保持连接、断开、等待重连间隔、重新连接”循环。
- 点击“自动重连”后，任务进入下方运行列表，显示连接状态、循环次数、错误、停止操作和虚机画面缩略图。
- 缩略图每秒拉取最新帧，并复用现有 `drawDesktopFrame` 解码及绘制逻辑。
- 所有测试任务共用一个 `thor-sidecar` 进程；任务区显示该进程的 PID，并每秒采样 CPU、RSS 内存和文件 I/O。
- Sidecar 使用 Node 内置的 `process.cpuUsage()`、`process.memoryUsage()` 和 `process.resourceUsage()` 返回累计值；前端按相邻采样差计算 CPU 百分比和 I/O 操作数/秒，保留最近 60 个点绘制滚动曲线。
- 新增 `process_metrics` JSON-RPC 方法和 `test_bench_process_metrics` Tauri 命令，没有引入新的运行时依赖。
- 使用 `AbortController` 停止等待和重连循环，并处理连接尚未完成时点击停止的竞态。
- 新增回归检查；前端 100 项、Sidecar 24 项和 Rust 45 项测试通过，`pnpm build` 与 Sidecar SEA 构建通过，并用新 EXE 实测 `process_metrics` 返回有效指标。

## 结论

测试台现在同时支持一次性保持连接和定时重连压力测试；运行任务区能在同一页面观察状态、最新虚机画面，以及共享测试进程的 PID 和资源趋势。

## 待办

- [ ] 在有真实登录态和可连接虚机的 Tauri 运行环境中人工确认缩略图刷新效果。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\pages\TestBenchPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\i18n.ts`
- 修改：`D:\Don\Projects\thor_terminal\sidecar-app\src\rpc.ts`
- 修改：`D:\Don\Projects\thor_terminal\sidecar-app\test\rpc.test.mjs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\sidecar.rs`
- 修改：`D:\Don\Projects\thor_terminal\src-tauri\src\lib.rs`
- 新建：`D:\Don\Projects\thor_terminal\test\testBench.test.ts`
- 新建：`sessions/2026-07-11-add-thor-terminal-test-bench-task-previews.md`
- 修改：`INDEX.md`

## 引用

- 无外部引用。
