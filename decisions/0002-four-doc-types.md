# 0002 — 四类文档分工：sessions / topics / decisions / references

- 状态：Accepted
- 日期：2026-05-08
- 相关 session：[2026-05-08-init-repo](../sessions/2026-05-08-init-repo.md)
- 相关 decision：[0001 使用 Markdown + Git](./0001-use-markdown-and-git.md)

## 背景
"把所有讨论都写下来"听上去简单，但如果不区分文档类型，会出现：

- 把临时讨论写进了"知识库"，污染长期内容
- 把稳定结论散落在多个会话里，无法被新 Agent 快速找到
- 外部链接到处复制粘贴，事实会漂移

需要一套足够简单、但能区分**过程 / 主题 / 结论 / 引用**的分类。

## 选项
1. **单一目录 `notes/`**，全靠文件名区分。简单，但无结构。
2. **按时间分目录**（year/month/day）。便于归档，但无法按主题聚合。
3. **按四类分目录：sessions / topics / decisions / references**。结构清晰，职责明确。
4. **完全模仿 Zettelkasten / PARA 等知识管理体系**。理论完善，但学习成本高，过度工程化。

## 决策
**采用方案 3：四类目录，职责单一。**

```diagram
╭──────────╮  讨论过程   ╭──────────╮  沉淀结论   ╭───────────╮
│ sessions │ ──────────▶ │ topics   │ ──────────▶ │ decisions │
╰─────┬────╯             ╰─────┬────╯             ╰─────┬─────╯
      │                        │                         │
      ╰──────── 引用 ─────────▶ references ◀─────────────╯
```

| 目录 | 角色 | 写作频率 | 是否会被反复修改 |
|---|---|---|---|
| `sessions/` | 一次会话的过程记录 | 每次讨论 | 几乎不修改（追加为主） |
| `topics/`   | 长期主题的当前认知 | 主题相关讨论后 | 反复迭代 |
| `decisions/`| 已收敛的结论 / 取舍 | 结论稳定时 | 只追加修订记录，不重写 |
| `references/`| 外部资料的中央登记处 | 引用外链时 | 偶尔补充摘录 |

## 理由
- **过程与结论分离**：sessions 是流水账，decisions 是稳定结论，新 Agent 只看 decisions 就能上手。
- **去重**：所有外链统一登记在 references/，避免散落在正文。
- **足够轻**：4 个目录 + 命名约定，没有强制 schema。
- **与编程仓库习惯一致**：decisions/ 借鉴 ADR（Architecture Decision Records）社区惯例，AI Agent 都熟悉这个模式。

## 影响
- 正面：结构清晰；新 Agent 可以"先看 decisions、再看 topics、按需翻 sessions"。
- 负面：写作时需要判断"这条该放哪类"，初期可能踩坑；解决方式：先放进当天的 session，事后再升格到 topic / decision。
- 后续：在 [AGENTS.md](../AGENTS.md) 中固化命名规范与流程。

## 修订记录
- 2026-05-08：创建。
