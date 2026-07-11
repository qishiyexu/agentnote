# Session: 让 ThorTerminal SSH 配置在原卡片就地编辑

- 日期：2026-07-11
- Agent：Codex（GPT-5）
- 参与者：Don、Codex
- 话题：调整 SSH 配置卡片的编辑位置。

## 进展

- 确认 SSH 编辑表单原本固定渲染在列表末尾的新增卡片中。
- 复用同一份配置表单；编辑时替换被点击的原卡片，新增卡片继续只负责新增配置。
- 切换新增与编辑状态时重置表单归属，避免新增按钮误保存为旧配置更新。
- 增加前端结构回归检查。

## 结论

SSH 配置点击“编辑”后会直接在原卡片位置编辑，不再跳到新的卡片。

## 验证

- `pnpm test`：97/97 通过。
- `pnpm build`：通过，仅保留既有 chunk size 提示。
- `git diff --check`：通过。

## 待办

- 无。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\pages\SshPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\test\sshSessionWindow.test.ts`
- 新增：`C:\Don\personal\agentnote\sessions\2026-07-11-edit-thor-terminal-ssh-config-in-place.md`

## 引用

- 无外部引用。
