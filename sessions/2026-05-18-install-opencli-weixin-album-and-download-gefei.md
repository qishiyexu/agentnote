# Session: 安装 opencli-weixin-album 并下载哥飞合集

- 日期：2026-05-18
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：（暂无，可后续提取为 topics/opencli-weixin-download.md）
- 相关 decision：（暂无）

## 话题
在 Windows 环境下安装 `SlowGrowth1314/opencli-weixin-album` 这个 OpenCLI 插件，并将微信公众号合集“养网站防老”的文章下载到 `D:\Don\articles\哥飞`。

## 进展
1. 按本仓库要求，先读取 `C:\Don\personal\agentnote\AGENTS.md`、`INDEX.md` 和最近 session，确认文档化流程。
2. 在工作区 `C:\Users\Don\Documents\Codex\2026-05-18\slowgrowth1314-opencli-weixin-album-https-github` 克隆 `SlowGrowth1314/opencli-weixin-album`。
3. 确认本机环境：Node.js `v23.6.0`、npm `6.14.18`、OpenCLI `1.7.22`。
4. 首次运行 `opencli plugin install github:SlowGrowth1314/opencli-weixin-album` 时遇到 `esbuild not found` 和 Windows 临时目录 rename 的 `EPERM`。
5. 尝试全局安装 `esbuild` 时因为 `C:\Program Files\nodejs\node_modules` 权限不足失败，随后改为安装到 `C:\Users\Don\.opencli` 用户目录。
6. 第二次插件安装成功，`opencli plugin list` 显示 `opencli-weixin-album @1.0.0` 已安装，`opencli weixin download-album --help` 可用。
7. `opencli doctor --verbose` 显示 daemon 正常运行在 `19825`，但 Browser Bridge 扩展未连接。已下载并解压 OpenCLI 扩展包到工作区 `opencli-extension`，但自动启动独立 Chrome 未能让扩展连接。
8. 运行插件下载用户提供的微信合集 URL，插件成功生成合集索引：`D:\Don\articles\哥飞\养网站防老\养网站防老.md`，共 247 篇，但正文下载受 Browser Bridge 未连接影响无法继续。
9. 为推进任务，编写本地补充脚本 `wechat-direct-downloader/download-wechat-album.cjs`：读取索引中的文章 URL，直连微信文章 HTML，提取正文，下载图片，转换为 Markdown，并回填索引中的本地路径。
10. 首次脚本下载成功 245 篇，有 2 篇因页面结构导致 `#js_content` 选择器未命中；随后增加备用正文块提取逻辑，补跑成功。

## 结论
1. `opencli-weixin-album` 已安装，命令 `opencli weixin download-album` 可用。
2. 本机 OpenCLI Browser Bridge 仍未连接；后续若要完全使用原插件的正文下载能力，需要在 Chrome 中启用 OpenCLI 扩展并让 `opencli doctor --verbose` 显示 `Extension: connected`。
3. 用户请求的微信合集已完成下载：
   - 输出目录：`D:\Don\articles\哥飞\养网站防老`
   - 索引文件：`D:\Don\articles\哥飞\养网站防老\养网站防老.md`
   - 合集文章：247 篇
   - 已下载 Markdown：247 篇
   - 索引本地路径回填：247 条
   - 图片文件：2317 个
4. 由于用户提供的原始微信 URL 包含较长会话参数，本 session 只记录合集名、输出路径和关键过程，不登记完整 URL。

## 待办
- [ ] 在 Chrome 中手动安装或启用 OpenCLI Browser Bridge 扩展，运行 `opencli doctor --verbose` 验证连接。
- [ ] 如未来继续批量下载微信合集，可复用本次生成的直连下载脚本，但要注意微信页面结构变化可能需要调整解析逻辑。
- [ ] 可后续提取一篇 topic，总结 Windows 下 OpenCLI 插件安装、Browser Bridge 连接和微信合集备份流程。

## 产出
- 新建：`references/opencli-weixin-album.md`
- 新建：`references/opencli.md`
- 新建：`sessions/2026-05-18-install-opencli-weixin-album-and-download-gefei.md`
- 修改：`INDEX.md`

## 引用
- [opencli-weixin-album](../references/opencli-weixin-album.md)
- [opencli](../references/opencli.md)
