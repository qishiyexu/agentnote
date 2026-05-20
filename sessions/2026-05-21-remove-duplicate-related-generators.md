# 2026-05-21 去掉 Elf Name Generator 重复的 Related generators

## Agent

Codex / GPT-5

## 参与者

Don + Codex

## 话题

去掉 Elf Name Generator 页面中重复出现的 Related generators 区块。

## 进展

- 删除 SEO 内容区右侧的 `Related generators` aside。
- 保留首屏里的内部链接，避免同一批 generator 页面链接重复出现。
- 将 SEO 内容区从两栏布局改为单栏正文布局。
- 清理不再使用的 `linkRail` 样式。

## 结论

内部链接集中保留在首屏即可。下方正文区不再重复同一组 generator 链接，页面会更简洁，视觉噪音更少。

## 待办

- 无。

## 产出

- 更新 `D:\Don\Projects\elfnamegenerator\components\SeoContent.tsx`
- 更新 `D:\Don\Projects\elfnamegenerator\app\globals.css`
- 本地验证 `npm run build`、`npm run lint`

## 引用

- 无外部引用。
