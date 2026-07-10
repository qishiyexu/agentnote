# INDEX — 全局索引

> 所有文档的入口。任何 Agent 接手仓库时，读完 [AGENTS.md](./AGENTS.md) 后从这里出发。
> **每次新增/重命名文档，请同步更新本文件。**

最后更新：2026-07-11

---

## Sessions（按时间倒序）

- [2026-07-11 让 ThorTerminal 服务器日志目录时间精确到秒](./sessions/2026-07-11-thor-terminal-server-log-directory-seconds.md) — 批量收集目录改为本地时间 `server_log_YYYY-MM-DD_HH-mm-ss`，避免同日任务共用一个目录。
- [2026-07-10 为 ThorTerminal 增加独立数据目录](./sessions/2026-07-10-add-thor-terminal-data-directory.md) — 保留原配置目录，新增独立数据目录，并统一日志下载、批量服务器日志和运行日志的落盘位置。
- [2026-07-10 为 ThorTerminal 添加服务器日志收集](./sessions/2026-07-10-add-thor-terminal-server-log-collection.md) — 从 SSH 配置选择服务器，维护带文件名通配符的日志项，并通过单次 SFTP 连接批量下载匹配日志。
- [2026-07-10 为 ThorTerminal 更换程序图标](./sessions/2026-07-10-set-thor-terminal-app-icon.md) — 选定雷神锤与终端提示符组合图标，生成全套桌面平台尺寸并验证嵌入 Windows 可执行文件。
- [2026-07-10 为 ThorTerminal SSH 标签添加通道复制和属性设置](./sessions/2026-07-10-add-thor-terminal-ssh-tab-copy-properties.md) — 右键 SSH 标签可复制独立通道，并即时设置当前终端的字体、字号和前后景颜色。
- [2026-07-10 为 ThorTerminal SSH 快捷命令添加分组参数](./sessions/2026-07-10-add-thor-terminal-ssh-snippet-groups.md) — 支持分组 CRUD、每组两个参数，以及执行快捷命令时展开 `{arg1}`、`{arg2}`。
- [2026-07-10 添加 ThorTerminal 跨平台 Portable 打包](./sessions/2026-07-10-add-thor-terminal-portable-packaging.md) — 将主程序及全部依赖汇总到单一目录，并修复 Windows 运行时系统命令连续闪现控制台窗口的问题。
- [2026-07-10 修复 ThorTerminal 多次连接后变慢和画面黑块](./sessions/2026-07-10-fix-thor-terminal-reconnect-black-frame.md) — 串行化旧会话清理、恢复被复用 context 的回调，并让原生渲染器等完整首帧后显示，避免二次连接卡住和局部黑屏。
- [2026-06-29 修复 storyboard 缩略图拖拽、持久化和预览](./sessions/2026-06-29-fix-storyboard-thumbnail-preview-persistence.md) — 统一缩略图保存到原图目录，修复拖拽上传、删除同步和重启后预览显示。
- [2026-06-25 用剪映合成《山海经》02 的 5 秒片段草稿](./sessions/2026-06-25-compose-shanhaijing-02-5s-jianying-draft.md) — 将 74 段 Agnes 5 秒视频和结尾图写入剪映本地草稿，保存在草稿库中未导出。
- [2026-06-24 用剪映合成《山海经》02 草稿并添加 narration 字幕](./sessions/2026-06-24-compose-shanhaijing-02-jianying-draft.md) — 将 38 段 Agnes 10 秒视频写入剪映本地草稿，按 75 条 narration 添加同步字幕并注册到本地草稿库。
- [2026-06-23 修复 ThorTerminal 启动后窗口不显示](./sessions/2026-06-23-fix-thor-terminal-startup-window.md) — 启动初始化时显式恢复、显示并聚焦主窗口，并用冷启动 Win32 探针确认进入前台。
- [2026-06-22 修复 Codex Windows sandbox helper 1223](./sessions/2026-06-22-fix-codex-windows-sandbox-helper-1223.md) — 移除本机 `[windows] sandbox = "elevated"`，避免 helper 走 UAC/提升链路触发 1223，并用真实环境 `codex doctor` 验证。
- [2026-06-22 持久化 ThorTerminal 窗口大小和位置](./sessions/2026-06-22-persist-thor-terminal-window-size.md) — 主窗口与桌面窗口按各自 label 保存尺寸和位置，并在下次创建时恢复。
- [2026-06-22 持久化 ThorTerminal 登录态](./sessions/2026-06-22-persist-thor-terminal-login.md) — 登录结果跨应用重启保留，主动退出时清除，且不写入 Git 工作区。
- [2026-06-22 渲染 ThorTerminal clink 返回画面](./sessions/2026-06-22-render-thor-terminal-clink-frames.md) — 接入 clink 图像帧，并修复连接、鼠标坐标及 Qt 键值输入。
- [2026-06-22 实现 ThorTerminal 桌面连接多窗口流程](./sessions/2026-06-22-implement-thor-terminal-desktop-multiwindow-flow.md) — 完成桌面会话独立窗口、重复连接聚焦、主动断开关窗、用户关窗自动断开及状态事件同步。
- [2026-06-22 实现 ThorTerminal JSON-RPC 通信协议](./sessions/2026-06-22-implement-thor-terminal-json-rpc.md) — 完成逐行 JSON-RPC 2.0、标准错误、通知语义及 Node/Rust 双端协议测试。
- [2026-06-22 用剪映合成《山海经》完整视频并添加字幕](./sessions/2026-06-22-compose-shanhaijing-video-with-captions.md) — 将 95 段相邻分镜视频顺序拼接为 7 分 55 秒成片，加入 95 条同步字幕并由剪映导出核验。
- [2026-06-22 安装 CapCut Mate](./sessions/2026-06-22-install-capcut-mate.md) — 在 Windows 本地完成 Python 与专用依赖安装，并通过 OpenAPI 请求验证服务可运行。
- [2026-06-21 搭建 ThorTerminal 基础 UI 框架](./sessions/2026-06-21-build-thor-terminal-ui-foundation.md) — 接入路由、共享布局、Tailwind/Shadcn、深浅主题和中英双语，并完成桌面交互验证。
- [2026-06-21 配置 ThorTerminal Sidecar IPC](./sessions/2026-06-21-configure-thor-terminal-sidecar-ipc.md) — 使用 Node SEA 与 Tauri Shell 打通 React、Rust、Sidecar 的完整 ping 链路，并在真实桌面窗口验证 pong。
- [2026-06-21 初始化 ThorTerminal Node.js Sidecar](./sessions/2026-06-21-initialize-thor-terminal-sidecar.md) — 建立 Node.js 20+ + TypeScript Sidecar workspace，并验证编译产物启动及 stdout 协议边界。
- [2026-06-21 初始化 ThorTerminal Tauri 项目](./sessions/2026-06-21-initialize-thor-terminal-tauri.md) — 完成 Tauri 2 + React + TypeScript 最小项目、工具链升级和桌面 debug 构建验证。
- [2026-06-21 整理 clink 内部异步 JSON 协议](./sessions/2026-06-21-document-clink-async-json-protocol.md) — 基于原生源码整理消息包络、回调时序、context 生命周期，并逐条映射双向命令/事件、参数与原生 API。
- [2026-06-21 补齐 clink Sidecar 自包含运行时](./sessions/2026-06-21-package-clink-sidecar-runtime.md) — 复制 Windows x64 第三方 DLL、GStreamer plugins、FFmpeg 和 VC++ runtime，并通过隔离路径的插件与 Node callback 验证。
- [2026-06-21 安装并启用 Ponytail 插件](./sessions/2026-06-21-install-enable-ponytail.md) — 安装 Ponytail 4.7.0，启用并信任两个会话钩子，修正 Codex 0.130.0 下的 Windows 命令兼容问题并完成端到端验证。
- [2026-06-21 生成 Windows x64 clink.node](./sessions/2026-06-21-build-clink-node.md) — 确认 deploy.sh 是自动编译入口，并记录 Git Bash 命令、x64 Release 输出路径及 x86 参数。
- [2026-06-21 分析 Rust 使用 clink.node](./sessions/2026-06-21-analyze-rust-clink-node-integration.md) — 实测 Node 加载和异步 callback，梳理导出 API、运行时闭包与 Buffer 边界，确认 Rust 应通过 JSON-RPC Sidecar 使用该模块。
- [2026-06-20 配置仅对 Codex 生效的本地代理](./sessions/2026-06-20-configure-codex-only-proxy.md) — 为 CLI 和桌面版建立进程级 Clash 代理启动器，不修改 Windows 用户或系统代理。
- [2026-06-20 添加《山海经》分镜可续跑脚本](./sessions/2026-06-20-add-resumable-shanhaijing-storyboard-runner.md) — 自动发现 JSONL 断点、退避重试、原子回写图片状态并同步生产日志。
- [2026-06-20 继续生成《山海经》5 秒分镜](./sessions/2026-06-20-continue-shanhaijing-storyboard.md) — 全片 scene-001 到 scene-096 已全部完成，并通过状态、路径与尺寸一致性核验。
- [2026-06-19 将 Name Dungeon 集成进 Elf Name Generator](./sessions/2026-06-19-integrate-name-dungeon.md) — 在每个生成结果旁增加带名字参数的游戏入口，并完成多语言、响应式与构建验证。
- [2026-06-19 继续生成《山海经》5 秒分镜](./sessions/2026-06-19-continue-shanhaijing-storyboard.md) — 已完成 scene-053 到 scene-058；当前 scene-059 因图像服务网络错误暂时阻塞。
- [2026-06-18 诊断 GitHub 443 连接失败](./sessions/2026-06-18-diagnose-github-443-connectivity.md) — 确认浏览器经 Clash 正常，而未配置代理的 Git 直连 GitHub 会卡住并超时。
- [2026-06-18 将 PowerShell 和 CMD 默认编码设为 UTF-8](./sessions/2026-06-18-set-powershell-cmd-utf8-default.md) — 修正 OneDrive 重定向导致的 PowerShell profile 路径错位，并让 CMD 默认进入代码页 65001。
- [2026-06-18 续跑《山海经》入门文案 5 秒分镜](./sessions/2026-06-18-resume-shanhaijing-intro-storyboard.md) — 补写 scene-025 到 scene-041 的实际完成状态，并确认下一断点为 scene-042。
- [2026-06-18 提升 Elf Name Generator 的 AdSense 审核准备度](./sessions/2026-06-18-improve-elf-name-generator-adsense-readiness.md) — 收缩索引到 15 个高价值 URL，补充核心页原创内容、命名指南、编辑透明度与 CMP 操作说明。
- [2026-06-18 审查 Elf Name Generator 的 AdSense 通过率](./sessions/2026-06-18-audit-elf-name-generator-adsense-readiness.md) — 识别多语言模板薄页为主要审核风险，并给出收缩索引、增加独特内容与补齐 CMP 的提交前方案。
- [2026-06-16 生成《山海经》入门文案 5 秒分镜首批图片](./sessions/2026-06-16-shanhaijing-intro-5s-storyboard.md) — 按约 5 秒一张图建立可续跑分镜记录，并生成开头 60 秒的 12 张图片。
- [2026-06-16 生成《山海经》怪物分镜与 Vidu 转场词](./sessions/2026-06-16-shanhaijing-monster-storyboard-vidu.md) — 基于九尾狐、烛龙、相柳口播文案生成 6 帧一致尺寸分镜图，并整理相邻帧转场提示词。
- [2026-06-15 生成《山海经》地理考察分镜视频](./sessions/2026-06-15-shanhaijing-geography-storyboard-video.md) — 基于四张古籍分镜使用 Agnes 生成 15 秒视频，并记录输出路径与校验结果。
- [2026-06-14 添加 Agnes 视频生成能力](./sessions/2026-06-14-add-agnes-video-capability.md) — 记录 `agnes_video_tool.py` 作为后续 agent 可复用的视频生成能力。
- [2026-06-13 生成《山海经》古籍展开动画尾帧](./sessions/2026-06-13-shanhaijing-tail-frame.md) — 基于首帧古籍图生成山海世界从书页中展开的尾帧资产。
- [2026-05-21 检查 Elf Name Generator 的 Vercel 部署适配](./sessions/2026-05-21-vercel-readiness-elf-name-generator.md) — 确认站点适合 Vercel，并补充部署配置、SEO metadata routes 与设计文档。
- [2026-05-21 去掉 Elf Name Generator 重复的 Related generators](./sessions/2026-05-21-remove-duplicate-related-generators.md) — 删除下方重复的相关生成器导航，保留首屏内部链接。
- [2026-05-20 优化 Elf Name Generator 首屏布局](./sessions/2026-05-20-improve-elf-generator-layout.md) — 调整生成器布局，让结果列表和 Generate 按钮在桌面与移动端都更显眼。
- [2026-05-20 补充 Elf Name Generator 子页面](./sessions/2026-05-20-add-elf-generator-pages.md) — 新增 KOTLC、dark、adult、neutral 和 elf names 页面，并修复 Next 16 动态路由参数。
- [2026-05-20 调整 Elf Name Generator 标签与页面控件](./sessions/2026-05-20-refine-elf-generator-ui.md) — 让名字标签动态化，移除 Count 控件，并去掉 FAQ 区块。
- [2026-05-19 制作 Elf Name Generator 英文网站](./sessions/2026-05-19-build-elf-name-generator-site.md) — 从空仓库实现 Next.js 英文精灵名生成器网站，并完成本地构建与视觉检查。
- [2026-05-18 总结哥飞“养网站防老”文章合集](./sessions/2026-05-18-summarize-gefei-yangwangzhan-articles.md) — 将 247 篇本地文章归纳为一份 Markdown 总摘要。
- [2026-05-18 安装 opencli-weixin-album 并下载哥飞合集](./sessions/2026-05-18-install-opencli-weixin-album-and-download-gefei.md) — 在 Windows 下安装 OpenCLI 微信合集下载插件，并完成“养网站防老”247 篇文章与图片的本地备份。
- [2026-05-08 Windows 11 下用 AGENTS.md 给 Amp 配置预加载系统 prompt](./sessions/2026-05-08-win11-amp-agents-md-config.md) — 在 Win11 下创建 `.config/amp/AGENTS.md` 并通过 `@` 引用本仓库 AGENTS.md，实现 Amp 跨会话自动加载系统 prompt。
- [2026-05-08 初始化仓库](./sessions/2026-05-08-init-repo.md) — 搭建目录结构、写下 AGENTS.md / README / 模板。

## Decisions

- [0001 仅使用 Markdown + Git 作为存储介质](./decisions/0001-use-markdown-and-git.md) — 不引入数据库 / 自定义工具，最大化可移植性。
- [0002 四类文档分工：sessions / topics / decisions / references](./decisions/0002-four-doc-types.md) — 明确每类文档的职责边界。

## Topics

- [Agnes 视频生成能力](./topics/agnes-video-generation.md) — 使用 `D:\Don\Projects\agnes\agnes_video_tool.py` 调用 Agnes AI 生成、轮询并下载视频。

## References

- [CapCut Mate](./references/capcut-mate.md) — 基于 FastAPI 的剪映草稿自动化 API 及其 Windows 本地安装方式。
- [Ponytail](./references/ponytail.md) — 为 Codex 提供持续任务推进、会话钩子和配套技能的插件。
- [Name Dungeon](./references/name-dungeon.md) — 支持通过 `?name=` 参数接收精灵名字的网页游戏。
- [Google AdSense eligibility and program policies](./references/google-adsense-eligibility-and-policies.md) — Google 对原创、高质量、合规内容与站点所有权的官方资格要求。
- [Google Search Console sitemap and indexing](./references/google-search-console-sitemap-and-indexing.md) — Google 官方 sitemap 提交、canonical URL 与实际收录检查说明。
- [Google Search scaled content policy](./references/google-search-scaled-content-policy.md) — Google 对规模化低价值页面的官方定义及本项目的风险映射。
- [opencli-weixin-album](./references/opencli-weixin-album.md) — OpenCLI 微信公众号合集文章下载插件。
- [OpenCLI](./references/opencli.md) — 命令行工具与 Browser Bridge 插件运行时。

---

## 模板

- [templates/session.md](./templates/session.md)
- [templates/decision.md](./templates/decision.md)
- [templates/topic.md](./templates/topic.md)
- [templates/reference.md](./templates/reference.md)

