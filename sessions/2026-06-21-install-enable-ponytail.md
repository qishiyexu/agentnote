# Session: 安装并启用 Ponytail 插件

- 日期：2026-06-21
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

在 Windows 上为 Codex 安装并启用 `DietrichGebert/ponytail`，确保插件技能和会话钩子在新会话中实际运行。

## 进展

1. 确认 Ponytail marketplace 已存在，并在 Codex 配置中启用 `ponytail@ponytail`。
2. 触发插件缓存安装，确认版本 `4.7.0` 的技能可以被新 Codex 会话发现。
3. 启用实验性 `plugin_hooks`，登记 SessionStart 和 UserPromptSubmit 两个钩子的可信哈希。
4. 发现 Codex CLI `0.130.0` 在 Windows 上执行了 POSIX `command`，未采用插件清单提供的 `commandWindows`。
5. 将插件缓存清单的两个通用命令改为等价 PowerShell 命令，更新可信哈希后重新验证。

## 结论

- Ponytail `4.7.0` 已安装并启用。
- 新 Codex 会话能够发现 Ponytail 技能。
- SessionStart 和 UserPromptSubmit 钩子均已信任，并在端到端验证中返回 `Completed`。
- 当前桌面会话需完全重启 Codex 并新建会话后加载插件。
- Windows 兼容修补位于插件缓存中，后续插件升级可能覆盖；届时应优先检查新版 Codex 是否已正确支持 `commandWindows`。

## 待办

- [ ] 升级 Codex 或 Ponytail 后复查两个钩子的信任状态与 Windows 命令。

## 产出

- 修改：`C:\Users\Don\.codex\config.toml`
- 修改：`C:\Users\Don\.codex\plugins\cache\ponytail\ponytail\4.7.0\hooks\claude-codex-hooks.json`
- 新建：`references/ponytail.md`
- 新建：`sessions/2026-06-21-install-enable-ponytail.md`
- 修改：`INDEX.md`

## 引用

- [Ponytail 仓库与安装文档](../references/ponytail.md)
