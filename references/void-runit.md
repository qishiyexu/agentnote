# Reference: Void Linux runit

- URL：<https://docs.voidlinux.org/config/services/index.html>
- 类型：文档
- 作者 / 来源：Void Linux Handbook
- 发布日期：未标注
- 访问日期：2026-07-11

## 一句话概括
Void Linux 官方的 runit 服务管理文档。

## 关键摘录

- Void 使用 runit 作为系统及服务管理器。
- 已启用服务通过 `/var/service` 目录管理，可使用 `sv status` 查询状态。

## 我们的备注
服务器报告在检测到 runit 时使用 `sv status /var/service/*` 采集服务状态。

## 被哪些文档引用

- [2026-07-11 为 ThorTerminal 增加服务器 HTML 报告](../sessions/2026-07-11-add-thor-terminal-server-report.md)
