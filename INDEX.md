# INDEX — 全局索引

> 所有文档的入口。任何 Agent 接手仓库时，读完 [AGENTS.md](./AGENTS.md) 后从这里出发。
> **每次新增/重命名文档，请同步更新本文件。**

最后更新：2026-06-14

---

## Sessions（按时间倒序）

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

- [opencli-weixin-album](./references/opencli-weixin-album.md) — OpenCLI 微信公众号合集文章下载插件。
- [OpenCLI](./references/opencli.md) — 命令行工具与 Browser Bridge 插件运行时。

---

## 模板

- [templates/session.md](./templates/session.md)
- [templates/decision.md](./templates/decision.md)
- [templates/topic.md](./templates/topic.md)
- [templates/reference.md](./templates/reference.md)

