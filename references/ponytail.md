# Reference: Ponytail

- URL：https://github.com/DietrichGebert/ponytail
- 类型：Codex 插件仓库 / 文档
- 作者 / 来源：DietrichGebert
- 发布日期：未知
- 访问日期：2026-06-21

## 一句话概括

Ponytail 是为 Codex 提供持续任务推进、会话钩子和配套技能的插件，支持通过 Codex plugin marketplace 安装。

## 关键摘录

仓库 README 给出的 Codex 流程是添加 marketplace、从 `/plugins` 安装、在 `/hooks` 信任钩子，并新建会话使插件生效。

## 我们的备注

本机安装版本为 `4.7.0`。Codex CLI `0.130.0` 没有采用清单中的 `commandWindows` 字段，因此把缓存清单里的通用 `command` 本地改为对应 PowerShell 命令，并为两个钩子登记当前可信哈希；新会话中两个钩子均显示 `Completed`。

## 被哪些文档引用

- [2026-06-21-install-enable-ponytail](../sessions/2026-06-21-install-enable-ponytail.md)
