# 2026-06-20 添加《山海经》分镜可续跑脚本

- Agent：Codex / GPT-5
- 参与者：Don、Codex
- 话题：为《山海经》5 秒分镜建立自动断点、重试和状态回写脚本

## 进展

- 核对用户提到的旧断点 `scene-059` 与磁盘真实状态，确认 `scene-059` 已完成，当前首个 `todo` 为 `scene-065`。
- 新增 `resume_storyboard.py`，每次从 `storyboard.jsonl` 自动寻找首个 `todo`。
- 生成器通过不经过 shell 的 JSON 命令数组接入；支持命令占位符和环境变量。
- 加入指数退避、可选抖动、最大重试次数、单实例文件锁、临时输出、PNG 签名校验与拒绝覆盖已有图片。
- 只有图片成功原子落盘后才把记录更新为 `done` 并填写 `output_path`；失败时保持 `todo`。
- 每次失败、等待和成功都会同步追加到 `production-log.md`。
- 兼容现有 `storyboard.jsonl` 的 UTF-8 BOM，并在回写时保持原编码特征。
- 更新输出目录 README，写明生成器接入方法和安全约束。
- 在隔离临时目录中验证首次失败后重试成功，以及重试耗尽后保持 `todo` 且不产生最终图片；真实已有图片未改动。

## 结论

分镜任务现可由脚本可靠续跑。脚本以 JSONL 实时状态为唯一断点依据，因此当前会从 `scene-065` 开始，也能用于任何停在 `scene-059` 等其它位置的同结构任务。

## 待办

- [ ] 配置实际图片生成后端的命令数组后，从 `scene-065` 继续生成。

## 产出

- 新增：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\resume_storyboard.py`
- 修改：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\README.md`
- 修改：`D:\Don\Projects\山海经\outputs\shanhaijing-intro-5s\production-log.md`

## 引用

- 无外部引用。
