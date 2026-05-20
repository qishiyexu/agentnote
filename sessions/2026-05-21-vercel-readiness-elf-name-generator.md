# 2026-05-21 检查 Elf Name Generator 的 Vercel 部署适配

## Agent

Codex / GPT-5

## 参与者

Don + Codex

## 话题

检查 Elf Name Generator 是否适合部署到 Vercel，并补充部署设计文档。

## 进展

- 确认项目是 Next.js App Router，页面均可静态预渲染，运行时不需要 API、数据库、文件写入或密钥。
- 将站点 URL 抽到 `NEXT_PUBLIC_SITE_URL`，用于 canonical、Open Graph、robots 和 sitemap。
- 新增 `robots.txt` 与 `sitemap.xml` metadata routes。
- 在 `package.json` 中声明 Node 版本范围，并新增 `npm run vercel:check` 作为部署前检查。
- 新增 `.vercelignore`，排除本地临时目录和运行时不需要的 SQLite 源库。
- 新增 Vercel 部署设计文档，说明架构、数据流、环境变量、构建命令和部署清单。
- 本地验证 `npm run vercel:check` 通过，生产服务下 `/`、`/robots.txt`、`/sitemap.xml`、`/dnd-elf-name-generator` 均返回 200。

## 结论

该站点适合部署到 Vercel。生产运行只依赖 Next.js 静态构建、浏览器端生成逻辑和 `data/elfnames.generated.json`，不依赖 SQLite 或服务器 API。SQLite 数据库应保留为本地内容源，不进入 Vercel 部署包。

## 待办

- 部署到正式域名后，在 Vercel 设置 `NEXT_PUBLIC_SITE_URL` 为最终域名。
- 如果后续更新 `data/elfnames.db`，先本地运行 `npm run export:elfnames` 并提交生成后的 JSON。

## 产出

- 更新 `D:\Don\Projects\elfnamegenerator\package.json`
- 更新 `D:\Don\Projects\elfnamegenerator\README.md`
- 更新 `D:\Don\Projects\elfnamegenerator\app\layout.tsx`
- 新增 `D:\Don\Projects\elfnamegenerator\app\robots.ts`
- 新增 `D:\Don\Projects\elfnamegenerator\app\sitemap.ts`
- 新增 `D:\Don\Projects\elfnamegenerator\lib\site.ts`
- 新增 `D:\Don\Projects\elfnamegenerator\.env.example`
- 新增 `D:\Don\Projects\elfnamegenerator\.vercelignore`
- 新增 `D:\Don\Projects\elfnamegenerator\docs\vercel-deployment-design.md`

## 引用

- 无外部引用写入本仓库文档。
