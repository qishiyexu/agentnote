# Session: 将 PowerShell 和 CMD 默认编码设为 UTF-8

- 日期：2026-06-18
- Agent：Codex / GPT-5
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

在 Windows 上让新启动的 Windows PowerShell 和 CMD 默认使用 UTF-8，避免中文及无 BOM UTF-8 文件出现乱码。

## 进展

- 检查发现 Windows PowerShell 的实际 profile 位于 OneDrive 重定向后的 `C:\Users\Don\OneDrive\文档\WindowsPowerShell\profile.ps1`。
- 原有 UTF-8 配置写在 `C:\Users\Don\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`，不是当前实际加载路径，因此没有生效。
- 在实际的 all-hosts profile 中设置控制台输入、控制台输出和原生命令管道为无 BOM UTF-8，并为常用文本文件 cmdlet 设置 UTF-8 默认值。
- 在当前用户的 `Command Processor\AutoRun` 注册表值前加入 `chcp 65001 >nul`，同时保留原有 Conda 启动钩子。
- 使用独立新进程验证：PowerShell 的输入、输出与原生命令管道均为 `utf-8`；CMD 的活动代码页为 `65001`；中文测试输出正常。

## 结论

PowerShell 配置必须写入系统当前实际解析出的 `$PROFILE.CurrentUserAllHosts`，不能假设“文档”目录一定是 `C:\Users\Don\Documents`。CMD 可通过当前用户的 `Command Processor\AutoRun` 切换到 UTF-8，但必须拼接而不是覆盖已有启动命令。

## 待办

- [ ] 无。

## 产出

- 修改：`C:\Users\Don\OneDrive\文档\WindowsPowerShell\profile.ps1`
- 修改：`HKCU\Software\Microsoft\Command Processor\AutoRun`
- 新建：`sessions/2026-06-18-set-powershell-cmd-utf8-default.md`
- 修改：`INDEX.md`

## 引用

- 无外部引用。
