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

## 连接窗口错过首帧修复

- 用户反馈点击“连接”后桌面窗口能进入等待状态，但云电脑画面没有出现。定位为首帧可能在桌面窗口 React 监听器挂载前已经通过 Tauri 事件发出，事件丢失后如果远端没有继续刷新，窗口会一直等待首帧。
- 修复方式保持最小：Sidecar 在成功拉取 `desktop-frame` 时按 `desktop_id` 缓存最后一帧，新增 JSON-RPC `get_frame`；Rust 透传为 `get_desktop_frame` Tauri 命令；桌面窗口挂载后先订阅实时帧，再主动拉取一次缓存帧。
- 断开连接时同步清理该桌面的帧缓存，避免重连显示旧画面。
- 验证通过：`pnpm --filter @thor-terminal/sidecar test`、`pnpm build`、`cargo test --manifest-path src-tauri\Cargo.toml`、`cargo fmt --manifest-path src-tauri\Cargo.toml -- --check`、`cargo clippy --manifest-path src-tauri\Cargo.toml --all-targets -- -D warnings`、`pnpm tauri build --debug --no-bundle`。
- 当前 debug 可执行文件：`D:\Don\Projects\thor_terminal\src-tauri\target\debug\thor-terminal.exe`。

## 连接接口请求参数修复
- 用户复测后仍提示“桌面连接已断开 / 请求参数不正确”，说明首帧缓存修复不是当前阻塞点，失败发生在 clink 配置下发前的业管 connect 接口。
- 对照官方客户端 `ArkConnectionService.qml` 与 `ArkDesktopService.qml` 后确认：`api/desktop/client/connect` / `connectMaster` 的 `connectData` 使用 axios `params`，不是请求 body 表单。
- ThorTerminal 原先在 `src-tauri/src/auth.rs` 中使用 `.form(&params)`，服务端收不到预期 query 参数，因此返回“请求参数不正确”。
- 修复为 `HashMap<String,String>` + `.query(&params)`，并将空 `vdCommand` 对齐官方 `JSON.stringify("")` 的字符串形态；保留服务端下发 `connectPath` / `statusPath`。
- 已重新执行：`pnpm --filter @thor-terminal/sidecar test`、`pnpm build`、`cargo test --manifest-path src-tauri\\Cargo.toml`、`cargo clippy --manifest-path src-tauri\\Cargo.toml --all-targets -- -D warnings`、`pnpm tauri build --debug --no-bundle`。
- 最新 debug 程序：`D:\Don\Projects\thor_terminal\src-tauri\target\debug\thor-terminal.exe`。

## Sidecar `clink.node` 打包修复

- 用户继续复测时出现 `clink.node` 不存在。直接运行 `src-tauri\binaries\thor-sidecar-x86_64-pc-windows-msvc.exe --clink-smoke` 复现：sidecar 只查找可执行文件相邻的 `src-tauri\binaries\lib\win32-x64`，而构建脚本只生成 exe，没有复制 native runtime。
- 修复 `sidecar-app/scripts/build-sea.mjs`：SEA 注入完成后，将 `sidecar-app/lib/<platform-arch>` 递归复制到 `src-tauri/binaries/lib/<platform-arch>`，保持与 `src/clink.ts` 的现有加载逻辑一致。
- 复验通过：`src-tauri\binaries\lib\win32-x64\clink.node` 存在；`--clink-smoke` 返回 `{"ok":true,...}` 并收到 clink callback；`pnpm --filter @thor-terminal/sidecar test` 4/4 通过；`pnpm tauri build --debug --no-bundle` 通过并再次复制 runtime。
- 已重启 debug 程序 `D:\Don\Projects\thor_terminal\src-tauri\target\debug\thor-terminal.exe`，后续点击“连接”会使用修复后的 sidecar。

## 客户端鼠标模式修复

- clink 底层默认请求 `CLINK_MOUSE_MODE_CLIENT`，因此不重复下发模式切换命令。
- 移除云桌面 Canvas 的 `cursor-none`，让客户端系统鼠标在窗口内保持可见。
- 拦截 Canvas 的 `contextmenu` 默认行为；现有右键按下/抬起仍通过 `mouse-input` 发送到云桌面。
- `pnpm build` 与 `git diff --check` 通过。
