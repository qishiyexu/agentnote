# Session: 为 Codex 配置 DeepSeek 自定义模型

- 日期：2026-06-19
- Agent：Codex / GPT-5
- 参与者：Don + Codex
- 相关 topic：无
- 相关 decision：无

## 话题

将 DeepSeek API 配置为 Codex 的用户级自定义模型提供商。

## 进展

- 检查 Codex 当前用户配置与最新配置手册，确认自定义 provider 应写入用户级 `~/.codex/config.toml`。
- 将当前模型切换为 `deepseek-v4-pro`，provider id 设为 `deepseek`。
- 配置 Base URL 为 `https://api.deepseek.com`，并通过 `env_key = "DEEPSEEK_API_KEY"` 从环境变量读取密钥。
- DeepSeek 使用 OpenAI 兼容的 Chat Completions 协议，因此配置 `wire_api = "chat"`。
- 运行 Codex 命令验证配置可被正常解析，退出码为 0。
- 检查发现当前进程、用户级环境和机器级环境均尚未设置 `DEEPSEEK_API_KEY`。

## 结论

Codex 已切换到 DeepSeek 自定义 provider，配置文件中未保存明文密钥。实际调用前仍需设置 `DEEPSEEK_API_KEY` 并重新启动 Codex，使新环境变量进入应用进程。

## 待办

- [ ] 用户设置 `DEEPSEEK_API_KEY` 环境变量。
- [ ] 重启 Codex 后发起一次模型请求，确认账号确实可访问 `deepseek-v4-pro`。

## 产出

- 修改：`C:\Users\Don\.codex\config.toml`
- 新建：`sessions/2026-06-19-configure-deepseek-custom-model-for-codex.md`
- 修改：`INDEX.md`

## 引用

- 无外部引用。
