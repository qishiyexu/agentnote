# Session: 生成《山海经》地理考察分镜视频

- 日期：2026-06-15
- Agent：Codex / GPT-5
- 参与者：Don + Codex
- 相关 topic：[Agnes 视频生成能力](../topics/agnes-video-generation.md)
- 相关 decision：无

## 话题

基于 `D:\Don\Projects\山海经\分镜\8.png` 和后续三张地理考察分镜，使用 Agnes 生成 15 秒《山海经》古籍风格视频。

## 进展

- 使用现有首帧 `8.png` 与新生成分镜 `8-geography-02.png`、`8-geography-03.png`、`8-geography-04.png` 作为镜头结构参考。
- 通过 `D:\Don\Projects\agnes\agnes_video_tool.py` 创建 Agnes 视频任务。
- 由于 Agnes 本地脚本只接收图片 URL，本次采用 text-to-video 提示词复现四段分镜节奏，而不是 keyframes URL 模式。
- 任务轮询期间多次遇到远端连接中断，改用单次查询加外层重试继续跟踪任务。

## 结论

Agnes 成功生成 15 秒视频，OpenCV 校验结果为 24 fps、361 帧、约 15.04 秒、1088x832。

## 待办

- [ ] 如后续有可公网访问的分镜图片 URL，可重新使用 `--keyframes` 生成更严格贴合分镜图的版本。

## 产出

- 视频：`D:\Don\Projects\山海经\视频\shanhaijing-geography-storyboard-15s.mp4`
- 预览帧：`D:\Don\Projects\山海经\视频\shanhaijing-geography-storyboard-15s-preview.png`

## 引用

- 无外部引用。
