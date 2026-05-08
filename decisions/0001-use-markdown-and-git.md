# 0001 — 仅使用 Markdown + Git 作为存储介质

- 状态：Accepted
- 日期：2026-05-08
- 相关 session：[2026-05-08-init-repo](../sessions/2026-05-08-init-repo.md)

## 背景
本仓库的目标是沉淀人与 AI Agent 的交互过程、结论与引用，并要求"换机器、换 Agent 都能无缝衔接"。
存储介质必须满足：

- **任何 Agent 都能直接读写**（不需要专用 SDK / 数据库驱动）
- **跨平台、跨设备可同步**
- **历史可追溯、可 diff、可 review**
- **零运维成本**

## 选项
1. **Markdown 文件 + Git 仓库**：纯文本，所有 Agent 都能直接读写；Git 提供版本控制与多机同步。
2. **专用知识库工具**（Notion / Obsidian / Logseq 等）：体验好，但绑定特定客户端，Agent 接入成本高。
3. **数据库 + 自建前端**：结构化最强，但开发与运维成本最高，违背"零运维"目标。

## 决策
**采用方案 1：Markdown + Git。**

## 理由
- Markdown 是 LLM 训练数据中最常见的文档格式，所有 Agent 都能高质量读写。
- Git 天然提供：多机同步、历史追溯、并发编辑（branch / merge）、code review 流程。
- 不绑定任何 GUI 工具：终端、VS Code、网页、手机客户端都能编辑。
- 与现有 AI 编程 Agent（Amp / Cursor / Cline / Claude Code …）完美契合。

## 影响
- 正面：零依赖、强可移植、易自动化、易跨工具协作。
- 负面：缺少富交互（关系图、全文检索需要外挂工具）；大规模时检索性能依赖 `grep` / `rg`。
- 后续：如有需要，可叠加 Obsidian / Logseq 作为**只读视图**，但权威源始终是 Git 仓库。

## 修订记录
- 2026-05-08：创建。
