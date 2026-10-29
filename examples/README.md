# 示例

以下是**自定义远程 MCP server** 的备用配置。如果你的 AI 应用连接器商店中已有 KuroKit，请优先使用该条目。

| 文件 | 适用情况 |
| --- | --- |
| [mcp.json](mcp.json) | 应用使用 JSON，并要求提供 token |
| [mcp-oauth.json](mcp-oauth.json) | 应用使用 JSON，且能自行打开 OAuth 登录页面 |
| [hermes.yaml](hermes.yaml) | Hermes 使用 YAML，并要求提供 token |
| [hermes-oauth.yaml](hermes-oauth.yaml) | Hermes 使用 YAML，且能自行打开 OAuth 登录页面 |

## 保存之前

1. 在 https://kurokit.com/dashboard/mcp 创建一个 token。
2. 用该 token 替换 `PASTE_TOKEN_FROM_DASHBOARD_MCP`。
3. 如果文件中已有其他 server，只需添加 `kurokit` 块，不要替换整个文件。
4. 重新加载 MCP，或重启应用。

切勿提交真实 token。

## 检查连接

打开一个新对话并询问：

```text
Call the KuroKit MCP tool listMonitors. Do not fetch any web pages.
Tell me how many monitors I have and their names.
```

如果助手列出了你的监控项，说明连接正常。

更多帮助：[快速开始](../docs/quickstart.md) 和 [故障排查](../docs/troubleshooting.md)。
