# Session: 生成 Windows x64 clink.node

- 日期：2026-06-21
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

确认 `C:\Don\code\srdcloud\client\clink` 在 Windows 上生成 `clink.node` 的正式构建入口、命令和输出路径。

## 进展

1. 核对 `clink-node/CMakeLists.txt`，确认 CMake 目标名为 `clinkProxy`，Windows 输出名被设置为 `clink.node`。
2. 核对 `clouddesktop-main/CMakeLists.txt`，确认需要启用 `BUILD_CLINK_NODE`。
3. 核对 `clouddesktop-script/script/clink/make.sh`、`deploy.sh` 和 `main.sh`，确认 `make.sh` 在 Windows 上只生成并打开 VS 工程，而 `deploy.sh` 会使用 NMake 自动完成 Release 编译。

## 结论

- 在 Git Bash 中进入 `client/clouddesktop-script/script/clink`，运行 `./deploy.sh -s Windows -a win64`。
- x64 Release 产物位于 `client/deploy/clink/Windows/win64/output/clink.node`，打包副本位于同级 `pack` 目录。
- PowerShell 中的裸 `bash` 当前指向 Windows/WSL 的 `bash.exe`，应明确使用 Git Bash，避免路径和 VS 环境初始化不一致。
- x86 构建将参数改为 `-a win32`。

## 待办

- [ ] 实际执行 x64 Release 构建，并用目标 Sidecar/Node 运行时做加载 smoke test。

## 产出

- 新建：`sessions/2026-06-21-build-clink-node.md`
- 修改：`INDEX.md`

## 引用

- 无外部引用；结论来自本地 clink 与 clouddesktop 构建脚本。
