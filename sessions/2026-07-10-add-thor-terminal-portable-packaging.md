# Session: 添加 ThorTerminal 跨平台 Portable 打包

- 日期：2026-07-10
- Agent：Codex
- 参与者：Don、Codex

## 目标

用一条跨平台命令生成可整体复制的 ThorTerminal 目录；主程序、Node Sidecar、clink 原生模块、动态库、GStreamer plugins 和解码资源都随包携带，不依赖源码目录。

## 实现

- 根目录新增 `pnpm package`，由纯 Node.js 标准库脚本执行 `tauri build --no-bundle`，再按当前 `process.platform-process.arch` 组装 `release/ThorTerminal-<version>-<platform>-<arch>/`。
- Portable 目录包含主程序、Tauri `externalBin` Sidecar、`target/release/lib` 原生运行时，以及 `tauri.conf.json` 中声明的全部 resources；脚本最后直接调用包内 Sidecar 的 `--clink-smoke`。
- Windows 通过系统 `cmd.exe` 调用 pnpm，规避 Node 24 直接 `spawn pnpm.cmd` 的 `EINVAL`。
- macOS 的 SEA 降级 wrapper 改为调用随包 Node runtime，不再要求目标机器预装 Node.js。
- Tauri 解码资源解析增加当前可执行文件目录回退，使平铺 Portable 目录在 Windows、macOS 和 Linux 都能找到随包资源。
- 若仓库缺少当前平台的 `sidecar-app/lib/<platform>-<arch>` 原生运行时，脚本在构建前明确失败；脚本支持各平台原生打包，不承诺跨平台交叉编译。

## 使用

```powershell
pnpm package
```

复制生成的整个目录，在其中直接运行 `ThorTerminal`；Windows 文件名为 `ThorTerminal.exe`。

## Windows 实机验证

- `pnpm package`：通过，release 构建及包内 clink callback smoke test 成功。
- 产物：`D:\Don\Projects\thor_terminal\release\ThorTerminal-0.1.0-win32-x64`。
- 清单：161 个文件，共 276.94 MiB；主程序、Sidecar、解码器、`clink.node`、动态库及 GStreamer plugins 均存在。
- 从 Portable 目录直接启动 `ThorTerminal.exe` 并观察 8 秒，进程保持运行。
- 根目录 `pnpm test`：72/72 通过。
- Sidecar `pnpm test`：24/24 通过。
- Rust `cargo test --lib`：41/41 通过。
- `node --check`、目标 Rust 文件 rustfmt 和 `git diff --check`：通过。

## 验收边界

本轮对 Windows x64 做了完整产物和启动验证。macOS/Linux 分支使用同一脚本与目录协议，但仍需在对应宿主机准备匹配的原生 runtime 后各跑一次 `pnpm package`；缺少 runtime 时脚本会拒绝产出不完整的包。

## 后续修复：启动时连续闪现控制台窗口

Windows release 主程序使用 GUI subsystem，但启动时读取设备标识会依次运行 `reg.exe`，登录阶段还会通过同一个 `command_output` 入口运行 `wmic.exe`、`getmac.exe` 和 `ipconfig.exe`。这些控制台程序原先没有设置 `CREATE_NO_WINDOW`，因此从 Portable EXE 启动时会短暂创建并关闭多个控制台窗口；Sidecar 由 `tauri-plugin-shell` 启动，库内原本已经使用该标志，不是弹窗来源。

修复在认证模块所有系统命令共用的 `command_output` 入口统一设置 Windows `CREATE_NO_WINDOW`，不改变其他平台。Win32 探针以 10 ms 间隔枚举新增可见 `ConsoleWindowClass`：旧包稳定捕获到 `reg.exe`，计数为 1；重打包后同一探针计数为 0。`cargo test --lib` 41/41、完整 `pnpm package` 和包内 clink smoke test 通过。
