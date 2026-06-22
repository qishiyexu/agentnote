# Session: 持久化 ThorTerminal 登录态

- 日期：2026-06-22
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

让 ThorTerminal 在登录后跨应用重启保留登录态，仅在用户主动退出时清除，并确保登录记录不进入 Git。

## 进展

1. 将账号密码登录和扫码登录的结果由 `sessionStorage` 改存到 Tauri WebView 的 `localStorage`。
2. Dashboard 启动时从持久化登录态加载真实桌面列表；已登录用户访问登录页时自动返回桌面列表。
3. 侧栏根据登录态显示“登录”或“退出登录”，主动退出只清除 `adminAuth` 并返回登录页。
4. 登录数据没有写入仓库文件；检查确认 Git 工作区未新增 token、session 或 auth 数据文件。

## 结论

- 沿用 WebView 原生本地存储即可满足单机持久化，不增加依赖或自定义配置文件。
- 登录结果可能包含 token，但只存在应用的 WebView 数据目录，不受 Git 跟踪。
- “记住账号”仍只控制账号文本；登录态本身在成功登录后始终保存，直到主动退出。

## 验证

- `pnpm build`
- `git diff --check`
- `git status --short` 与敏感文件名扫描

## 待办

- [ ] 后续实现 3.2 Token 自动刷新机制。

## 产出

- 修改：`D:\Don\Projects\thor_terminal\src\App.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\LoginPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\pages\DashboardPage.tsx`
- 修改：`D:\Don\Projects\thor_terminal\src\i18n.ts`
- 修改：`D:\Don\Projects\thor_terminal\CHANGELOG.md`
- 修改：`D:\Don\Projects\thor_terminal\STATUS.md`

## 引用

- 本次沿用项目现有架构与浏览器原生存储能力，未新增外部引用。
