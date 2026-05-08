# agentnote

沉淀人与 AI Agent 之间交互的知识仓库。

> 所有讨论进展、结论、引用都以 **Markdown 文档** 形式持久化在 Git 中，确保换机器、换 Agent（Claude、GPT、Gemini、Amp、Cursor……）都能无缝衔接。

---

## 为什么要有这个仓库

和 AI Agent 的对话往往散落在各个客户端、各个会话窗口里：
- 关掉窗口 → 上下文消失
- 换一台机器 → 历史不在
- 换一个 Agent → 完全从零开始
- 同一个问题反复讨论 → 结论无法复用

**agentnote** 把这些交互"工程化"：把过程、结论、引用、待办都落到文件里，纳入 Git 版本管理，任何 Agent 拉一下仓库就能接着干。

---

## 目录结构

```
agentnote/
├── README.md           # 本文件：项目说明
├── AGENTS.md           # 给 AI Agent 的工作约定（必读）
├── INDEX.md            # 全局索引：所有 session / decision / topic 的入口
│
├── sessions/           # 单次会话日志（按时间）
│   └── YYYY-MM-DD-<slug>.md
│
├── decisions/          # 已落地的结论 / 决策（ADR 风格）
│   └── NNNN-<slug>.md
│
├── topics/             # 长期主题笔记 / 知识库（按主题）
│   └── <slug>.md
│
├── references/         # 外部引用：文章、论文、链接、原文摘录
│   └── <slug>.md
│
└── templates/          # 上述各类文档的模板
    ├── session.md
    ├── decision.md
    ├── topic.md
    └── reference.md
```

四种文档的关系：

```diagram
╭──────────╮  讨论过程   ╭──────────╮  沉淀结论   ╭───────────╮
│ sessions │ ──────────▶ │ topics   │ ──────────▶ │ decisions │
╰─────┬────╯             ╰─────┬────╯             ╰─────┬─────╯
      │                        │                         │
      ╰──────── 引用 ─────────▶ references ◀─────────────╯
```

- **sessions/**：原始对话/讨论的过程记录（一次会话一篇）
- **topics/**：围绕某个主题不断迭代的笔记（一个主题一篇，会持续更新）
- **decisions/**：经过讨论得到的稳定结论 / 取舍 / 原则（一旦写定不轻易改）
- **references/**：所有外部引用都登记在此，方便复用与追溯

---

## 快速开始

### 1. 开一次新会话
复制模板，填写当天讨论：
```
cp templates/session.md sessions/2026-05-08-<slug>.md
```

### 2. 沉淀一个结论
当某个讨论收敛，把结论独立成 ADR：
```
cp templates/decision.md decisions/0001-<slug>.md
```

### 3. 维护主题笔记
对长期话题，在 `topics/` 中维护一个不断迭代的文档。

### 4. 登记引用
所有外链、论文、文章原文摘录放在 `references/`，其它文档通过相对链接引用。

### 5. 更新索引
在 [INDEX.md](./INDEX.md) 顶部加一行新条目，让其它 Agent 一眼能找到。

### 6. 提交
```
git add .
git commit -m "session: <date> <topic>"
git push
```

---

## 给 AI Agent 的话

**任何 Agent 接手时，请先读 [AGENTS.md](./AGENTS.md) 和 [INDEX.md](./INDEX.md)。**
