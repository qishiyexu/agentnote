# Reference: Google Search Console sitemap and indexing

- URL: <https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap>
- URL: <https://support.google.com/webmasters/answer/7451001?hl=en>
- 类型：官方文档
- 作者 / 来源：Google Search Central / Search Console Help
- 访问日期：2026-06-18

## 一句话概括

Sitemap 应列出希望进入搜索结果的 canonical URL，可通过 Search Console 提交或在 robots.txt 中声明；提交只是发现提示，不保证 Google 一定收录。

## 本项目的用法

- sitemap 仅列出英文主页、12 个已有独立内容的生成器页、文章索引、12 篇指南和法律页。
- robots.txt 声明 sitemap 地址并允许正常抓取。
- Search Console 的所有权验证 token 通过 `NEXT_PUBLIC_GOOGLE_SITE_VERIFICATION` 注入。
- 提交 AdSense 前仍需在 Search Console 的 URL Inspection 与 Page indexing 报告中确认主要 URL 的实际收录状态；代码构建通过不能替代这一步。

## 被哪些文档引用

- [2026-06-18 提升 Elf Name Generator 的 AdSense 审核准备度](../sessions/2026-06-18-improve-elf-name-generator-adsense-readiness.md)
