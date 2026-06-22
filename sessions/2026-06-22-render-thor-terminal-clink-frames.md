# Session: 渲染 ThorTerminal clink 返回画面

- 日期：2026-06-22
- Agent：Codex
- 参与者：Don、Codex

## 主题

在桌面连接多窗口流程上接入真实云桌面配置，并把 `clink.node` 返回的图像帧渲染到独立桌面窗口。

## 完成内容

- 调用管理端 `connect` / `connectMaster` 与状态轮询接口，生成 clink 所需的真实连接配置。
- Sidecar 保存原生连接 context，处理 `image-change`，通过 `_ClinkGetImage` 读取 64 字节图像参数和像素缓冲区。
- 将帧通过 JSON-RPC Base64 通知传给前端，在 Canvas 中渲染 RGBA、NV12、I420 和 Y444。
- 桌面窗口增加首帧等待、渲染失败与断开状态，并按 desktop id 隔离多窗口帧事件。
- 更新 API 规范、状态和变更日志，补充 Sidecar 图像帧测试。

## 结论

- 原生图像参数结构固定为 16 个 32 位整数，共 64 字节，不能使用不完整的模拟参数。
- 原生 context 只保留在 Sidecar 内部，不暴露给 Rust 或前端。
- 首版使用现有 stdio JSON-RPC 传输 Base64 帧，不增加依赖；只有实测高帧率形成瓶颈后，再考虑共享内存或独立二进制通道。

## 验证

- `pnpm --filter @thor-terminal/sidecar test`：4/4 通过，包含真实 `clink.node` 回调和模拟 RGBA 帧拉取。
- `pnpm build`：通过。
- `cargo test --manifest-path src-tauri\Cargo.toml`：5/5 通过。
- `cargo clippy --manifest-path src-tauri\Cargo.toml --all-targets -- -D warnings`：通过。
- `cargo fmt --manifest-path src-tauri\Cargo.toml -- --check`：通过。
- `pnpm sidecar:build`：通过，生成的 SEA 可执行文件 ping 返回 `pong`。
- `pnpm tauri build --debug --no-bundle`：通过。

## 验收边界

当前环境没有已登录的云桌面账号，因此没有实际建立远端会话并目视确认真实首帧；管理端配置、原生缓冲读取、帧序列化、前端解码渲染和打包程序均已验证。

## 连接回访修复

用户实测发现点击“连接”没有云桌面窗口和画面。回访后确认首版存在三个问题：

- 桌面窗口在管理端连接和 clink `run` 完成后才创建，连接失败时不会出现窗口，首帧也可能早于前端监听器。
- clink 可能在 `run` 返回 context 的同一调用周期先发出 `image-change`，Sidecar 尚未建立 context 到 desktop id 的映射时会丢弃该帧。
- 管理端 `connect` 是 `application/x-www-form-urlencoded` POST，首版错误地把参数放在 URL 查询串；同时没有采用桌面下发的 `connectApiUrl.connectPath/statusPath`。

修复后，点击“连接”会立即创建独立桌面窗口；Sidecar 暂存过早到达的 `image-change` 并在 context 绑定后拉帧；管理端连接参数改为表单请求体并优先使用下发接口路径。使用现有登录态真实点击状态为 `OK` 的桌面，已目视确认独立窗口立即出现，并捕获到旧请求返回的“请求参数不正确”；改为表单请求体后的 Rust 热更新清除了临时登录态，因此远端首帧仍需重新登录后复验，不能记为已目视通过。

修复后验证：Sidecar 4/4、Rust 6/6、前端生产构建均通过，开发版运行于 `http://localhost:1420/`。

## 后续

- 重新登录后完成连接、看到首帧、断开和多窗口并发验收。
- 接入鼠标、键盘及窗口尺寸变化事件。
- 仅在性能数据证明 stdio Base64 成为瓶颈时升级帧传输方式。

## 产物

- `D:\Don\Projects\thor_terminal\src-tauri\target\debug\thor-terminal.exe`
- `D:\Don\Projects\thor_terminal\src\lib\desktopFrame.ts`
- `D:\Don\Projects\thor_terminal\src\pages\DesktopWindowPage.tsx`
- `D:\Don\Projects\thor_terminal\sidecar-app\src\rpc.ts`
