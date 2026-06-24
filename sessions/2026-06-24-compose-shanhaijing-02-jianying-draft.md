# Session: 用剪映合成《山海经》02 草稿并添加 narration 字幕

- 日期：2026-06-24
- Agent：GPT-5 Codex
- 参与者：Don + GPT-5 Codex
- 相关 topic：[Agnes 视频生成能力](../topics/agnes-video-generation.md)
- 相关 reference：[CapCut Mate](../references/capcut-mate.md)

## 话题
把《山海经》02 的 Agnes 10 秒分镜视频组成剪映本地草稿，并用 `storyboard.jsonl` 里的 `narration` 做同步字幕/口播稿文本。

## 进展
先确认 `agnes-10s/video-progress.jsonl` 中 38 段 Agnes 视频全部完成，再读取 75 条原始分镜 narration。

通过 CapCut Mate 的本地 Python 环境直接调用草稿生成逻辑，把草稿写入剪映本地草稿根目录：
`C:\Users\Don\AppData\Local\JianyingPro\User Data\Projects\com.lveditor.draft\202606241440098d010599`。

为避免本地素材被 API 当作网络 URL 下载，脚本将 38 个 mp4 复制进草稿的 `assets/videos/`，再把这些本地素材加入视频轨。

字幕按原始 5 秒分镜排布：scene-001 到 scene-074 每条 5 秒；最后一个 Agnes 视频是 scene-074 到 scene-075 的 10 秒过渡，因此 scene-075 的 narration 放在 370s 到 380s，避免重复 scene-074 文案。

## 结论
当 Agnes 以相邻分镜生成 10 秒视频、原始 narration 仍是 5 秒粒度时，剪映草稿里视频轨应按 10 秒连续拼接，字幕轨仍按原始 narration 的 5 秒节点对齐。

奇数分镜尾部如果用最后两帧补一条 10 秒视频，应让倒数第二条 narration 保持在上一条 10 秒视频的后半段，最后一条 narration 覆盖最后 10 秒视频，避免文案重复。

进度应写入 `agnes-10s/capcut-draft-progress.jsonl`，关键阶段包括 inputs_loaded、draft_created、video_asset_copied、videos_added、captions_added、draft_registered、verified。

## 待办
- [ ] 如需要真实语音口播，再补 TTS 音频生成与音频轨同步。

## 产出
- 新建：`C:\Users\Don\OneDrive\私人资料库\Projects\山海经\02\create_jianying_draft.py`
- 新建：`C:\Users\Don\OneDrive\私人资料库\Projects\山海经\02\agnes-10s\capcut-draft-progress.jsonl`
- 新建剪映草稿：`C:\Users\Don\AppData\Local\JianyingPro\User Data\Projects\com.lveditor.draft\202606241440098d010599`
- 修改：`C:\Users\Don\AppData\Local\JianyingPro\User Data\Projects\com.lveditor.draft\root_meta_info.json`

## 引用
- [CapCut Mate](../references/capcut-mate.md)
