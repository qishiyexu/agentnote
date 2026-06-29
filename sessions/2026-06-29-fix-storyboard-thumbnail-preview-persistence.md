# Session: 修复 storyboard 缩略图拖拽、持久化和预览

- 日期：2026-06-29
- Agent：GPT-5
- 参与者：Don + GPT-5
- 相关 topic：无
- 相关 decision：无

## 话题
修复 storyboard 中缩略图拖拽上传无响应、上传失败、重启后不显示预览的问题，并收敛为只使用原图目录保存。

## 进展
先排查前端拖拽和上传链路，再核对 Tauri 命令参数和文件保存路径。过程中确认过一次 Rust/JS 参数名不一致、预览路径不适合直接用 `file://`、以及上传后没有自动落盘导致重启失效。

## 结论
缩略图不再使用单独文件夹，统一保存到 `originals/`，文件名跟分镜 ID 对齐；上传和删除都会同步更新磁盘；重启后通过读取原图生成 `data:` 预览，保证缩略图框里可见。

## 待办
- [ ] 继续观察真实运行时是否还有拖拽区域命中问题。
- [ ] 如后续再改图像存储策略，优先同步更新文档说明。

## 产出
- 新建：`C:\Don\personal\agentnote\sessions\2026-06-29-fix-storyboard-thumbnail-preview-persistence.md`
- 修改：`D:\Don\Projects\storyboard\app\app.js`
- 修改：`D:\Don\Projects\storyboard\src-tauri\src\main.rs`
- 修改：`D:\Don\Projects\storyboard\src-tauri\tauri.conf.json`
- 修改：`D:\Don\Projects\storyboard\README.md`
- 修改：`D:\Don\Projects\storyboard\docs\storyboard-json-design.md`

## 引用
- 无
