# Session: 添加 Agnes 视频生成能力

- 日期：2026-06-14
- Agent：Codex / GPT-5
- 参与者：Don + Codex
- 相关 topic：[Agnes 视频生成能力](../topics/agnes-video-generation.md)
- 相关 decision：无

## 话题

把 `D:\Don\Projects\agnes\agnes_video_tool.py` 记录为 agentnote 中可复用的视频生成能力。

## 进展

- 读取本仓库 `AGENTS.md`、`INDEX.md` 和最近 session，确认新增可复用能力应写入 topic，并同步 session 与索引。
- 确认 `D:\Don\Projects\agnes\agnes_video_tool.py` 存在，并阅读脚本的命令行参数。
- 整理 `generate`、图片生成视频、关键帧动画、帧数约束、默认输出目录和密钥处理注意事项。

## 结论

后续 agent 遇到视频生成、image-to-video、首尾帧动画生成等需求时，应优先查看并使用 [Agnes 视频生成能力](../topics/agnes-video-generation.md) 中记录的本地脚本能力。

## 待办

- [ ] 如脚本参数或 Agnes API 行为变化，更新 topic 中的命令模板。

## 产出

- 新建：`topics/agnes-video-generation.md`
- 新建：`sessions/2026-06-14-add-agnes-video-capability.md`
- 修改：`INDEX.md`

## 引用

- 无外部引用。
