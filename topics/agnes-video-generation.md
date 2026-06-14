# Agnes 视频生成能力

- 最后更新：2026-06-14
- 相关 sessions：[2026-06-14 添加 Agnes 视频生成能力](../sessions/2026-06-14-add-agnes-video-capability.md)
- 相关 decisions：无

## 用途

当用户希望 agent 直接生成视频、把首尾帧做成动画、基于图片生成视频，或需要调用 Agnes AI 视频接口时，优先使用本地脚本：

`D:\Don\Projects\agnes\agnes_video_tool.py`

脚本是 Agnes AI 视频生成的 CLI helper，封装了创建任务、查询结果、等待完成和下载视频。

## 常用命令

在 PowerShell 中运行：

```powershell
python D:\Don\Projects\agnes\agnes_video_tool.py generate `
  --prompt "视频描述" `
  --width 1152 `
  --height 768 `
  --num-frames 121 `
  --frame-rate 24 `
  --out-dir "D:\Don\Projects\agnes\output\agnes-video"
```

图片生成视频：

```powershell
python D:\Don\Projects\agnes\agnes_video_tool.py generate `
  --prompt "让画面自然动起来，保持原图主体和风格" `
  --image "https://example.com/input.png" `
  --out-dir "D:\Don\Projects\agnes\output\agnes-video"
```

关键帧动画：

```powershell
python D:\Don\Projects\agnes\agnes_video_tool.py generate `
  --prompt "从第一张图自然过渡到第二张图" `
  --keyframes "https://example.com/start.png" "https://example.com/end.png" `
  --out-dir "D:\Don\Projects\agnes\output\agnes-video"
```

## 参数要点

- 默认模型：`agnes-video-v2.0`
- 默认输出目录：`D:\Don\Projects\agnes\output\agnes-video`
- `--num-frames` 必须满足 `8n + 1`，例如 `81`、`121`、`241`、`441`，且不超过 `441`。
- `generate` 会创建任务、轮询结果并下载视频；如果只需要分步调试，可用 `create`、`get`、`wait`。
- `--payload` 可以传入 JSON 对象，覆盖或补充请求体字段。

## 注意事项

- 不要把 API key 写进 agentnote 文档或对话结论；需要密钥时使用环境变量 `AGNES_API_KEY` 或脚本参数。
- 运行前确认输入图片 URL 可被 Agnes API 访问；本地文件通常需要先上传或换成可访问 URL。
- 生成完成后，把实际产出的 `.mp4` 路径写入对应 session，便于后续追溯。
