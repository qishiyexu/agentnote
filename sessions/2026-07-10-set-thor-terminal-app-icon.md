# Session: 为 ThorTerminal 更换程序图标

- 日期：2026-07-10
- Agent：Codex（GPT-5）
- 参与者：Don、Codex
- 话题：从 5 个候选方案中选定雷神锤终端图标，并应用到 ThorTerminal 桌面程序。

## 进展

- 选用候选方案 1：青色雷神锤与终端提示符组合，深色背景，保留少量黄色光标点缀。
- 将生成原图保存为 `D:\Don\Projects\thor_terminal\src-tauri\app-icon.png`，不再依赖 Codex 的生成图片目录。
- 复用 Tauri 内置 `icon` 命令，重新生成 Windows、macOS、Linux 桌面打包所需的 PNG、ICO、ICNS 和 Windows Store 尺寸。
- 未保留本项目不使用的 Android、iOS 图标产物。

## 结论

ThorTerminal 的程序图标统一使用“雷神锤 + 终端提示符”方案，颜色继续贴合现有深色界面、青色主色和黄色强调色。

## 验证

- `pnpm tauri build --debug --no-bundle`：通过。
- 从 `src-tauri/target/debug/thor-terminal.exe` 提取嵌入图标并目视确认：与所选方案一致。
- `git diff --check`：通过。

## 待办

- 无。

## 产出

- 新增：`D:\Don\Projects\thor_terminal\src-tauri\app-icon.png`
- 更新：`D:\Don\Projects\thor_terminal\src-tauri\icons\`

## 引用

- 无外部引用。
