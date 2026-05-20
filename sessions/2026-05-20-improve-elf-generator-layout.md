# 2026-05-20 优化 Elf Name Generator 首屏布局

## Agent

Codex / GPT-5

## 参与者

Don + Codex

## 话题

优化 Elf Name Generator 的首屏布局，让结果列表和 Generate 按钮更显眼。

## 进展

- 将生成器面板拆成左右两列：左侧为选项，右侧为结果列表。
- 右侧结果区增加独立高亮容器，标题改为 `Elf names / Your results`。
- 将主操作按钮改为显眼的 `Generate`，并放在结果区标题旁边。
- 移动端调整顺序：标题简介之后优先显示结果区和 Generate，再显示选项与相关页面链接。
- 将 hero 图片改为 CSS 背景，避免图片层级覆盖交互内容。
- 收紧移动端文本、结果卡和复制按钮的伸缩规则，减少小屏横向溢出风险。

## 结论

生成类 SEO 页面首屏应先暴露可用结果和主操作，而不是让页面链接或完整筛选控件占据移动端首屏。筛选项仍保留，但在小屏下应让位给结果浏览和刷新生成。

## 待办

- 发布前可再用真实手机浏览器检查一次 390px 以下的小屏排版。

## 产出

- 更新 `D:\Don\Projects\elfnamegenerator\components\Generator.tsx`
- 更新 `D:\Don\Projects\elfnamegenerator\app\globals.css`
- 本地验证 `npm run build`、`npm run lint`

## 引用

- 无外部引用。
