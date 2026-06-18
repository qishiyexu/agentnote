# Session: 诊断 GitHub 443 连接失败

- 日期：2026-06-18
- Agent：Codex / GPT-5
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

诊断浏览器可以打开 GitHub，但 Git/终端提示 GitHub 443 无法连接的问题。

## 进展

- 确认 `github.com` 可正常解析为 `20.205.243.166`，hosts 文件中没有 GitHub 覆盖项。
- Windows 当前用户代理已启用，地址为 `127.0.0.1:7890`；该端口由 Clash for Windows 的 `clash-win64.exe` 监听。
- Git 的 system/global/local/worktree 配置均未设置 HTTP/HTTPS 代理，相关代理环境变量也不存在，因此 Git 默认不沿用浏览器的系统代理。
- 强制直连 GitHub 时请求长时间卡住并超时；通过 `127.0.0.1:7890` 请求 GitHub 页面约 0.6 秒完成。
- `git ls-remote` 强制通过 Clash 代理时 1.8 秒成功返回；强制直连的重复测试持续到诊断命令超时。

## 结论

故障不是 GitHub 443 整体不可达，也不是 DNS、hosts、证书或 Clash 未运行。根因是浏览器与 Git 使用了不同网络链路：浏览器使用 Clash 系统代理，而 Git 未配置代理并尝试不稳定的 GitHub 直连，因而出现连接 443 超时。当前未修改系统或 Git 配置。

## 待办

- [ ] 如需修复，为 Git 配置 `http://127.0.0.1:7890` 代理，并在 Clash 关闭时同步取消该配置。

## 产出

- 新建：`sessions/2026-06-18-diagnose-github-443-connectivity.md`
- 修改：`INDEX.md`

## 引用

- 无外部引用。
