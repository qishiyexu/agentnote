# Session: 生成《山海经》入门文案 5 秒分镜首批图片

- 日期：2026-06-16
- Agent：Codex / GPT-5
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

基于一篇解释《山海经》是什么的长口播文案，按朗读速度约每 5 秒一张图，建立可续跑的分镜生成记录，并生成首批 12 张图片。

## 进展

- 读取本仓库 `AGENTS.md` 与 `INDEX.md`，确认新增文档需要同步登记。
- 参考最近的《山海经》分镜与视频生成记录，沿用古籍、竹简、矿物颜料、水墨山海世界、青铜纹饰的视觉方向。
- 在项目目录 `D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\` 建立可续跑任务记录。
- 将文案开头约 60 秒切成 12 个 5 秒分镜，覆盖现代人初遇《山海经》、游戏/短视频/朋友传闻入口、书的身份追问、世界草图、古人面对未知、以及古代档案突然转入怪兽的第一处转折。
- 使用图像生成能力逐张生成 `scene-001.png` 到 `scene-012.png`，并复制保存到项目目录。
- 生成 `contact-sheet-scenes-001-012.jpg` 作为首批分镜总览。
- 2026-06-17 追加：将完整口播扩展为全片 `storyboard.jsonl`，共 96 条 5 秒分镜，覆盖约 8 分钟；前 12 条保持 `done`，后 84 条为 `todo`。

## 结论

首批 12 张分镜已经落盘，完整 `storyboard.jsonl` 已补齐到 `scene-096`。每条都记录了时间码、旁白片段、视觉摘要、生成提示词、状态和输出路径。后续新任务可以直接读取该 JSONL，从 `scene-013` 之后继续生成图片并延续同一画风。

## 待办

- [x] 继续从 `scene-013` 开始扩展后续文案分镜。
- [ ] 继续从 `scene-013` 开始生成后续图片。
- [ ] 如需进入视频制作，基于已生成图片继续整理相邻帧转场提示词。

## 产出

- 新建：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\README.md`
- 新建：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\storyboard.jsonl`
- 新建：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\production-log.md`
- 新建：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\images\scene-001.png` 到 `scene-012.png`
- 新建：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\contact-sheet-scenes-001-012.jpg`

## 引用

- 无外部引用。
