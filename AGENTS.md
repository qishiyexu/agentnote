# AGENTS.md — 给所有 AI Agent 的工作约定

> 任何 Agent（Claude / GPT / Gemini / Amp / Cursor / Cline …）接手本仓库时，**必须先读完本文件**，再读 [INDEX.md](./INDEX.md)，再开始工作。

本仓库的核心目的：**让人和 AI 之间的所有讨论、结论、引用都以文档形式持久化，跨机器、跨 Agent 无缝衔接**。

---

## 0. 铁律

1. **写下来 > 记在脑子里**：任何稍有价值的讨论结论、设计取舍、外部引用，立刻落到对应文档中并 `git commit`。
2. **链接 > 复制**：跨文档引用一律使用相对路径链接，不要复制粘贴大段内容（避免事实漂移）。
3. **可追溯**：每个结论都要能回溯到产生它的 session 或 reference。
4. DO NOT send optional commentary

---

## 1. 接手仓库后的标准流程

```diagram
╭──────────────╮   ╭──────────────╮   ╭───────────────────╮   ╭──────────╮
│ 读 AGENTS.md │──▶│ 读 INDEX.md  │──▶│ 读最近 1-2 个     │──▶│ 开始工作 │
│              │   │              │   │ session + 相关    │   │          │
│              │   │              │   │ topic / decision  │   │          │
╰──────────────╯   ╰──────────────╯   ╰───────────────────╯   ╰─────┬────╯
                                                                     │
                          ╭──────────────────────────────────────────╯
                          ▼
       ╭───────────────────────────────╮      ╭──────────────────────╮
       │ 新建/更新 session / topic /   │ ───▶ │ 更新 INDEX.md +      │
       │ decision / reference 文档     │      │ git commit & push    │
       ╰───────────────────────────────╯      ╰──────────────────────╯
```

---

## 2. 文档的写作规范

### 2.1 sessions/ — 会话日志

- 文件名：`YYYY-MM-DD-<slug>.md`，例如 `2026-05-08-init-repo.md`
- 一次会话一篇，**当天就写**，不要攒。
- 必含字段（见 [templates/session.md](./templates/session.md)）：
  - `Agent`：用了哪个 Agent + 模型
  - `参与者`：人 + Agent
  - `话题`：一句话概括
  - `进展 / 结论 / 待办`：分别成段
  - `产出`：本次新增/修改了哪些文档
  - `引用`：链接到 references/ 中的条目

### 2.2 decisions/ — 决策记录（ADR）

- 文件名：`NNNN-<slug>.md`，4 位递增编号，例如 `0001-use-markdown-only.md`
- 一旦写定，**只追加修订记录，不重写历史**。
- 字段：背景 / 选项 / 决策 / 理由 / 影响 / 状态（Proposed / Accepted / Superseded by NNNN）。

### 2.3 topics/ — 主题笔记

- 文件名：`<slug>.md`，例如 `prompt-engineering.md`
- 一个主题一篇，**会被反复编辑**。
- 顶部维护"最后更新时间"和"相关 sessions / decisions"链接。

### 2.4 references/ — 外部引用

- 文件名：`<slug>.md`，例如 `anthropic-prompt-caching.md`
- 字段：标题 / URL / 作者 / 访问日期 / 关键摘录 / 我们的备注。
- **所有外链都先在这里登记**，其它文档引用时链到此文件，而不是直接放裸 URL。

## 2.5 文章生成

- 如果主体是生成文章，要使用humanizer这个skill去AI味。

---

## 3. 索引（INDEX.md）维护

- 每次新增/重命名文档，**同步更新 [INDEX.md](./INDEX.md)**。
- INDEX.md 按四个分区组织：Sessions（倒序）、Decisions、Topics、References。
- 每条目格式：`- [标题](相对路径) — 一句话摘要`

---

## 4. 提交规范（Conventional Commits 简化版）

- 如果需要提交，先读COMMIT.md

---

## 5. 反模式（不要做）

- ❌ 把结论只写在对话里、不落到文档里
- ❌ 大段复制外部内容到正文（请放进 references/ 并链接）
- ❌ 重命名/删除已有 decision，应新增一条 `Superseded by` 关系
- ❌ 一次 commit 涉及多个无关话题
- ❌ 写完不更新 INDEX.md
- ❌ 写完不 push
