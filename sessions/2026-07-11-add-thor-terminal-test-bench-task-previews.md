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
- 每个测试任务独占一个 `thor-sidecar` 进程；各任务卡分别显示自己的 PID，并每秒采样对应进程的 CPU、RSS 内存和文件 I/O。
- Sidecar 使用 Node 内置的 `process.cpuUsage()`、`process.memoryUsage()` 和 `process.resourceUsage()` 返回累计值；前端按相邻采样差计算 CPU 百分比和 I/O 操作数/秒，保留最近 60 个点绘制滚动曲线。
- 新增 `process_metrics` JSON-RPC 方法；Rust 为测试任务按桌面 ID 维护独立 `SidecarManager`，画面、连接和资源采样都走任务专属命令，没有影响虚拟应用仍在使用的共享连接链路。
- 停止任务时从任务进程表移除对应 Sidecar 并终止进程；循环重连只断开会话并保留该任务进程。
- 使用 `AbortController` 停止等待和重连循环，并处理连接尚未完成时点击停止的竞态。
- 修复知识库任务填写 `5` 毫秒时“自动重连”按钮被禁用：移除没有产品依据的 100 毫秒下限，改为接受 `1` 到 `86400000` 毫秒的正数。
- 新增回归检查；前端 100 项、Sidecar 24 项和 Rust 45 项测试通过，`pnpm build` 与 Sidecar SEA 构建通过，并用新 EXE 实测 `process_metrics` 返回有效指标。
- 补充正数持续时间的按钮可用性回归检查；开发窗口确认知识库行按钮已启用，完整前端 101 项测试与生产构建通过。
- 将测试任务状态和 `AbortController` 提升为进程内共享存储，并通过 `useSyncExternalStore` 订阅；离开测试台时不再停止任务或回收任务 Sidecar，返回后任务卡、状态、缩略图和进程指标轮询会重新挂载。
- 增加跨页面保留任务的回归检查；完整前端 102 项测试与生产构建通过。
- 为 CPU、内存和 I/O 曲线补充纵轴数值、横轴起止时间，并提供 1、5、15 分钟的时间跨度选择；每张曲线同时显示当前值和所选时间窗口内的平均值。
- 指标历史扩展到 900 个采样点以覆盖最长 15 分钟；新增坐标、跨度和平均值回归检查，完整前端 103 项测试、Sidecar 24 项测试和生产构建通过。
- 审查后让历史采样上限由最大时间跨度自动推导，将曲线组件及纵轴参数改为准确命名，并移除连接持续时间的 24 小时上限，保持任意正数都有效。
- ThorTerminal 提交：`8be96ef feat: 完善测试台任务监控`。
- 时间跨度增加 1 小时选项；由于历史上限由最大跨度推导，采样容量同步扩展为 3600 个点。
- 每台虚机增加“显示画面”勾选项。任务连接成功后由 Tauri 创建独立画面窗口，以 100 毫秒间隔复用任务专属 `test_task_get_frame` 和 `drawDesktopFrame` 刷新；进入断开流程、停止任务或连接报错时销毁窗口。
- 画面窗口使用独立 `/test-task-display/:desktopId` 路由和 `desktop-test-*` 窗口标签，不接管正常云桌面会话，也不改变每个测试任务独占 Sidecar 的结构。
- 新增 1 小时时间跨度和画面窗口生命周期回归检查；完整前端 105 项测试、生产构建和 Rust 45 项测试通过。
- ThorTerminal 提交：`f40f5d7 feat: 增加测试任务画面窗口和一小时曲线`。
- 运行时出现 `Command open_test_task_window not found`，检查确认源码和 `invoke_handler` 已注册，但正在运行的 `thor-terminal.exe` 启动于提交前，二进制中不包含该命令。停止旧主进程及其 Sidecar、执行 `pnpm tauri build --debug --no-bundle` 后重启；新进程二进制已包含命令，前端 105 项测试再次通过。

## 结论

测试台现在同时支持一次性保持连接和定时重连压力测试；每项任务有独立进程和独立资源趋势，停止任务会同步回收该进程。连接持续时间只要是正数即可启动，不再因小于 100 毫秒而静默禁用按钮。测试任务在主进程存活期间不依赖测试台页面生命周期，切换页面后仍会继续运行并在返回时恢复显示。资源曲线具备时间和数值坐标，可切换 1、5、15 分钟或 1 小时观察窗口并读取窗口平均值。勾选“显示画面”后，测试会话连接期间显示独立实时画面窗口，断开时窗口同步关闭。

## 待办

- [ ] 在有真实登录态和可连接虚机的 Tauri 运行环境中人工确认缩略图刷新效果。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\pages\TestBenchPage.tsx`
- 新建：`D:\Don\Projects\thor_terminal\src\pages\TestTaskDisplayPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\router.tsx`
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
