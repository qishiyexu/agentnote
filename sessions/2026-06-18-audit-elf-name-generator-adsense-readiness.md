# Session: 审查 Elf Name Generator 的 AdSense 通过率

- 日期：2026-06-18
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：（暂无）
- 相关 decision：（暂无）

## 话题

检查 <https://www.elf-name-generators.com/> 当前是否适合申请 Google AdSense，并制定提高通过率的内容、索引、信任与合规改造方案。

## 进展

1. 实查线上首页、About、Contact、Privacy Policy、sitemap、robots.txt，均正常返回 200。
2. 核对源码，确认已加载 AdSense 站点审核脚本，`ads.txt` 包含正确发布商 ID，并具备 About、Contact、Privacy Policy、Terms、robots 与 sitemap。
3. sitemap 共有 17 个主 URL、104 个 hreflang alternate URL；站点实际形成 8 种语言乘 13 个工具入口的较大索引面。
4. 英文首页约 562 个空白分词，英文生成器页约 404 个；但 12 个英文子页复用同一组五段说明，主要差异只有标题、简介和默认筛选器。
5. 多语言子页复用同一组三段短说明，部分 meta description 直接等于页面标题，信息增益比英文页更低。
6. 对照 Google 官方 AdSense 资格要求与 Search 规模化内容政策，判定主要风险是低价值、薄页和规模化模板内容，而不是缺少法律页面或明显违禁内容。
7. 本地 `npm run build` 因 `.next/dev/types/validator.ts` 残留对已不存在 `/articles/[slug]` 的引用而失败；工作树本身干净，需清理构建缓存后复验。

## 结论

当前直接提交 AdSense 不属于高把握状态。站点的工具可用、基础信任页齐全、内容家庭安全，这是正面项；但 104 个多语言入口中大量页面只有相同工具、相同模板说明与关键词替换，容易被判断为低价值内容。经验性估计当前通过概率约为 30%–45%，该区间不是 Google 官方数据或保证。

提交前优先改造：

1. 暂时只保留英文首页和 4–6 个真正有独特用途的英文页可索引；其余重复页先 `noindex` 并从 sitemap/hreflang 移除。多语言版本应等完成母语级独特内容后分批开放。
2. 将每个保留页改成独立资源，而不是共用五段模板。每页至少加入该场景专属的命名规则、音节/发音规律、20–50 个静态精选示例、适用角色、常见错误、原创 FAQ 和交叉链接。
3. 新增 3–5 篇真正解决问题的常青指南，例如精灵命名体系、家族名设计、同一文化角色群命名、如何判断名字是否适合 DND 角色；文章应由人审校，并展示作者/编辑方法与更新时间。
4. 强化 About：说明站点负责人或编辑身份、生成算法与数据来源、人工审校流程、联系方式和独立站声明。Privacy Policy 中已有广告说明，但应把纯文本 Google URL 改成可点击链接。
5. 接入适用于 EEA、英国和瑞士的 Google 认证 CMP；在同意状态确定前按 Google Consent Mode/AdSense 要求处理广告请求。
6. 清理 `.next` 缓存后执行 `npm run vercel:check`，确认构建、类型检查和 lint 全部通过；再检查 Search Console 索引覆盖、手动措施、移动体验和 Core Web Vitals。
7. 内容与索引改造上线后，先让 Google 重新抓取并观察 2–4 周；确保核心页面已经收录、站点有自然访问与真实互动后再申请。Google 没有公布最低流量或最低文章数，不应机械追求数量。

## 待办

- [ ] 确定第一批保留索引的 5–7 个英文页面。
- [ ] 为保留页分别撰写独特内容规格并实现。
- [ ] 将其他语言和重复页设为 `noindex`，同步收缩 sitemap 与 hreflang。
- [ ] 新增原创指南、作者/编辑方法与 CMP。
- [ ] 修复干净构建并完成上线后复查，再提交 AdSense。

## 产出

- 新建：`sessions/2026-06-18-audit-elf-name-generator-adsense-readiness.md`
- 新建：`references/google-adsense-eligibility-and-policies.md`
- 新建：`references/google-search-scaled-content-policy.md`
- 修改：`INDEX.md`

## 引用

- [Google AdSense eligibility and program policies](../references/google-adsense-eligibility-and-policies.md)
- [Google Search scaled content policy](../references/google-search-scaled-content-policy.md)

