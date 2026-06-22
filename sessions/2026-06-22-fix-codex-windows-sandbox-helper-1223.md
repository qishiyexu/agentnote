# Session: 修复 Codex Windows sandbox helper 1223

- 日期：2026-06-22
- Agent：GPT-5 Codex
- 参与者：Don + GPT-5 Codex

## 话题
解决本机 Codex Windows sandbox helper 因提升启动链路触发 `ShellExecuteExW ... 1223` 的问题。

## 进展
- 按仓库规则先读取 `AGENTS.md` 和 `INDEX.md`。
- 检查 `C:\Users\Don\.codex\config.toml`，发现 `[windows] sandbox = "elevated"`。
- 检查 `C:\Users\Don\.codex\.sandbox\sandbox.2026-06-22.log`，确认 Codex 会反复启动 `codex-windows-sandbox-setup.exe` 和 copied command runner。
- 备份原配置到 `C:\Users\Don\.codex\config.toml.bak-20260622-sandbox-1223`，然后移除 `[windows] sandbox = "elevated"`。
- 在非沙箱环境运行 `C:\Users\Don\.codex\.sandbox-bin\codex.exe doctor` 验证真实 `CODEX_HOME=C:\Users\Don\.codex`。

## 结论
`1223` 是 Windows 的取消/拒绝启动错误；本机触发点是 Codex 配置把 Windows sandbox helper 放进 `elevated` 提升链路。删除该配置后，Codex 仍保持 `restricted fs + restricted network · approval OnRequest`，没有关闭沙箱。

## 待办
- [ ] 如果未来再次出现 1223，先检查 `C:\Users\Don\.codex\config.toml` 是否又恢复了 `[windows] sandbox = "elevated"`。

## 产出
- 修改：`C:\Users\Don\.codex\config.toml`
- 新增备份：`C:\Users\Don\.codex\config.toml.bak-20260622-sandbox-1223`
- 新建：`sessions/2026-06-22-fix-codex-windows-sandbox-helper-1223.md`
- 修改：`INDEX.md`