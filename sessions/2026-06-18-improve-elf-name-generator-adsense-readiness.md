# 2026-06-18 提升 Elf Name Generator 的 AdSense 审核准备度

## 目标

按同日审核结论改造 `D:\Don\Projects\elfnamegenerator`，降低模板薄页风险，补足原创内容、编辑透明度、隐私与同意管理说明。

## 完成内容

- 将可索引生成器页收缩为首页与 6 个内容较完整的英文核心页；其余英文长尾页及全部翻译页保留可访问，但设为 `noindex, follow`。
- 将多语言页 canonical 指向对应英文页，并从 sitemap 与 hreflang 中移除未成熟翻译页。
- 为核心页增加独立使用方法、编辑挑选示例、发音提示、FAQ 与编辑方法说明。
- 新增 Naming guides 文章索引，以及 3 篇原创指南：命名体系、家族/家名、群像角色命名。
- 扩写 About 与 Privacy，说明内容审核方法、名字生成机制、纠错渠道、广告和 Google 认证 CMP 的账号侧配置步骤。
- sitemap 现只包含 15 个高价值 URL。

## 验证

- `npm run lint` 通过。
- `npm run build` 通过，Next.js 静态生成 116 个路由。
- Chrome/Playwright 检查桌面首页、文章页与 390px 移动端核心页，无横向溢出或遮挡。
- 生成器 Generate 按钮会刷新结果。
- 非核心页和翻译页输出 `noindex, follow`；翻译页 canonical 指向英文对应页。
- 本地预览唯一 404 是 `/_vercel/insights/script.js`，属于脱离 Vercel 环境时的 Analytics 代理缺失，不是站内资源遗漏。

## Vercel Node 与 lockfile 修正

- 将 `package.json` 的 Node 引擎从 `>=22 <24` 改为 `24.x`，与 Vercel Project Settings 的 24.x 对齐，避免部署时覆盖设置并回退到 Node 22。
- 使用 npm 11 将 `package-lock.json` 从 lockfile v1 升级为 v3，消除 Vercel 安装阶段的 `npm warn old lockfile`。
- 重新执行 npm 11 的 clean install、lint 和 production build，均通过；生产依赖 `npm audit --omit=dev` 为 0 个漏洞。

## 仍需账号侧操作

代码不能代替 AdSense 后台操作。正式送审前，需要在 AdSense 的 Privacy & messaging 中发布适用于 EEA、英国和瑞士用户的 Google 认证 CMP 消息，并确认站点所有权、广告代码和政策中心没有待处理问题。

## 结论

站点已从大量近似模板页调整为少量重点索引页加原创指南的结构，技术和内容层面的审核准备度明显提高。是否通过仍由 Google 的实际审核、站点流量与账号状态决定，不能保证通过。
