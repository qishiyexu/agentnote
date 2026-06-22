# Session: 用剪映合成《山海经》完整视频并添加字幕

- 日期：2026-06-22
- Agent：GPT-5 Codex
- 参与者：Don + Agent
- 相关 topic：无
- 相关 decision：无

## 话题
学习本地 CapCut Mate 文档，把《山海经》95 段相邻分镜视频拼接成完整视频，并加入逐段同步字幕后用剪映导出。

## 进展
1. 阅读 `http://localhost:30000/docs` 及 `create_draft`、`add_videos`、`add_captions`、`save_draft`、`gen_video` 文档。
2. 按文件名顺序加入 `scene-001-to-scene-002.mp4` 至 `scene-095-to-scene-096.mp4`，每段 5 秒，共 475 秒。
3. 从 `storyboard.jsonl` 取前 95 条旁白，按对应的 5 秒区间添加白字黑边字幕。
4. 生成 CapCut Mate 草稿 `202606220019068143d57a`，再由本机剪映专业版 10.7 打开并导出。
5. 将成片归位到项目输出目录，并用 Windows 媒体属性核验文件。

## 结论
- 成片包含 95 段视频和 95 条烧录字幕。
- 成片时长 7 分 55 秒，分辨率 1672×940，帧率 30 fps，文件大小 575,413,147 字节。
- `storyboard.jsonl` 的第 96 条旁白没有对应的 `scene-096-to-scene-097.mp4`，因此未加入本次仅基于 `videos` 目录素材的成片。

## 待办
- [ ] 如补生成第 96 段转场视频，再追加最后一条旁白并重新导出。

## 产出
- 新建：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\shanhaijing-intro-complete-subtitles.mp4`
- 新建剪映草稿：`202606220019068143d57a`

## 引用
- [CapCut Mate](../references/capcut-mate.md)

## 追加：字幕下移并缩小
- 按用户要求未重新导出，只更新剪映草稿箱里的草稿 `202606220019068143d57a`。
- 95 条字幕字号从 12 缩小到 6，字幕段统一移动到下方位置 `transform.y = 0.78`。
- 同步更新 CapCut Mate 输出草稿与剪映本地草稿目录；本地原文件保留 `pre-bottom-small-*` 备份。

## 追加：修复草稿箱不可见/打不开
- 用户反馈草稿箱没有更新视频后，确认原因是只改了草稿文件，未正确让剪映草稿箱索引稳定刷新；剪映本地草稿列表依赖 `root_meta_info.json`。
- 新建并注册可见草稿 `山海经-字幕下方小字`，路径为 `C:\Users\Don\AppData\Local\JianyingPro\User Data\Projects\com.lveditor.draft\山海经-字幕下方小字`。
- 修复过程中发现负数 `transform.y` 会触发剪映“草稿内容已损坏”，改回 0 到 1 范围内的合法位置；最终 95 条字幕统一为 `transform.y = 0.08`，字号保持 6。
- 最终文件侧校验通过：草稿已注册、`draft_content.json`/`draft_info.json` 可解析、95 段视频、95 条字幕、总时长 07:55；并重启剪映主进程刷新缓存。
