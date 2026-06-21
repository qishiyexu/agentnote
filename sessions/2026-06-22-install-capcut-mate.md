# Session: 安装 CapCut Mate

- 日期：2026-06-22
- Agent：Codex（GPT-5）
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

将 `Hommy-master/capcut-mate` 安装到 `D:\Don\Projects\capcut-mate`，并验证 Windows 本地服务可以运行。

## 进展

1. 克隆仓库到目标目录，当前提交为 `1601c93`。
2. 使用本机 `uv 0.11.23` 创建 `.venv` 并执行 `uv sync`。
3. 按项目中文 README 的 Windows 步骤执行 `uv pip install -e .[windows]`。
4. 短暂启动 `uv run main.py`，请求 `http://127.0.0.1:30000/openapi.json`，收到 HTTP 200。
5. 验证 `fastapi`、`win32api` 和项目包均可导入；测试后关闭服务并清理临时日志。

## 结论

- CapCut Mate 已完整安装到目标目录。
- Windows 专用依赖已安装，虚拟环境位于 `.venv`。
- API 服务能够监听 30000 端口并正常返回 OpenAPI 文档。
- 日常启动命令为 `uv run main.py`，API 文档地址为 `http://localhost:30000/docs`。

## 待办

- 无。

## 产出

- 安装：`D:\Don\Projects\capcut-mate`
- 新建：`references/capcut-mate.md`
- 新建：`sessions/2026-06-22-install-capcut-mate.md`
- 修改：`INDEX.md`

## 引用

- [CapCut Mate](../references/capcut-mate.md)
