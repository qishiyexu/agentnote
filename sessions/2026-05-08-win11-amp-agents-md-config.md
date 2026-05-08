# Session: Windows 11 下用 AGENTS.md 给 Amp 配置预加载系统 prompt

- 日期：2026-05-08
- Agent：Perplexity（Claude Sonnet 4.6）
- 参与者：qishiyexu + Perplexity Agent
- 相关 topic：（暂无，可后续提取为 topics/amp-agents-md.md）
- 相关 decision：[0001-use-markdown-and-git](../decisions/0001-use-markdown-and-git.md)

## 话题

在 Windows 11 环境下，如何让 Amp 在每次回答问题前自动预先读取 agentnote 仓库里的 AGENTS.md（即本仓库），实现跨会话的系统 prompt 持久化。

## 进展

1. 用户询问 Amp 如何添加「回答问题之前预先读取的系统 prompt」。
2. 确认 Amp 遵循 AGENTS.md 规范，会从工作目录及全局配置路径自动加载 `AGENTS.md` / `AGENT.md` / `CLAUDE.md`，无需手动每次指示。
3. 用户说明环境为 Windows 11，发现 `C:\Users\<用户名>\.config\` 下没有 `amp` 目录——这是正常现象，需要手动创建。
4. 给出 PowerShell 创建目录和文件的完整命令，以及在全局 `AGENTS.md` 中通过 `@` 引用本仓库 `AGENT.md`/`AGENTS.md` 的方式。
5. 用户提供 GitHub 仓库地址 `https://github.com/qishiyexu/agentnote`，Agent 读取仓库规范后，按 AGENTS.md 要求写入本次 session。

## 结论

1. **Amp 自动读取路径（Windows 11）**：
   - 当前工作目录及其父目录：`AGENTS.md` / `AGENT.md` / `CLAUDE.md`
   - 全局配置：`C:\Users\<用户名>\.config\amp\AGENTS.md`（需手动创建目录）
   - 越接近当前目录的文件优先级越高。

2. **Windows 11 下创建全局配置**（PowerShell）：
   ```powershell
   cd $HOME
   mkdir .config -Force
   mkdir .config\amp -Force
   New-Item -Path .config\amp\AGENTS.md -ItemType File -Force
   code $HOME\.config\amp\AGENTS.md
   ```

3. **全局 AGENTS.md 内容示例**（把本仓库接入）：
   ```md
   # Global instructions (Windows)

   @~/code/agentnote/AGENTS.md

   # 说明：
   # 1. 先加载 agentnote 仓库的规则。
   # 2. 如果当前仓库目录里也有 AGENTS.md / AGENT.md，则更具体的就近文件优先生效。
   ```
   其中 `~` 解析为 `C:\Users\<用户名>`，路径跨平台可复用。

4. **远程 GitHub 仓库不能直接当预读源**：必须先 `git clone` 到本地，再通过 `@` 引用。

5. **优先级链**：`全局 $HOME/.config/amp/AGENTS.md` → `仓库根目录 AGENTS.md` → `子目录 AGENTS.md`，越近越优先，可覆盖上层规则。

## 待办

- [ ] 将 agentnote 仓库 clone 到本地（例如 `C:\Users\<用户名>\code\agentnote`）
- [ ] 创建 `C:\Users\<用户名>\.config\amp\AGENTS.md` 并写入 `@~/code/agentnote/AGENTS.md` 引用
- [ ] 验证 Amp 在某个测试仓库下是否按预期自动读取 agentnote 规则
- [ ] 如有需要，提取为 topics/amp-agents-md.md 主题笔记

## 产出

- 新建：`sessions/2026-05-08-win11-amp-agents-md-config.md`
- 修改：`INDEX.md`（新增本 session 条目）

## 引用

_（暂无外链需登记。相关技术规范见 https://github.com/agentmd/agent.md 和 https://github.com/agentsmd/agents.md，如需后续引用请在 references/ 中登记。）_
