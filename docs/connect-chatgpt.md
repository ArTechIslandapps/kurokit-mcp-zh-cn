# 将 KuroKit 连接到 ChatGPT 或 Codex

ChatGPT 和 Codex 对远程 MCP 的支持取决于你的方案、地区和客户端版本。如果你的应用没有提供自定义连接器，请改用其他支持 MCP 的客户端，或直接使用 KuroKit 控制台。

官方且持续更新的指南：https://kurokit.com/docs/mcp

## 如果应用的连接器商店中有 KuroKit

1. 打开应用的连接器商店或 MCP 应用商店。
2. 搜索 KuroKit。
3. 添加并完成登录。

有该条目时优先使用这种方式。你无需粘贴 token。

## 如果应用支持自定义连接器

添加一个远程 MCP server：

- 网址：`https://mcp.kurokit.com/mcp`
- 请求头：`Authorization: Bearer <your token>`

在 https://kurokit.com/dashboard/mcp 创建 token。请把它粘贴到应用的 MCP 设置中，而不是聊天框里。

如果应用要求提供 JSON 配置，请使用 [examples/mcp.json](../examples/mcp.json) 并替换其中的占位符。

## 检查连接

打开一个新对话并询问：

```text
Call the KuroKit MCP tool listMonitors. Do not fetch any web pages.
Tell me how many monitors I have and their names.
```

如果助手列出了你的监控项，说明连接正常。

## 如果无法使用自定义连接器

你仍可通过 https://kurokit.com 上的控制台使用 KuroKit，或连接其他 MCP 客户端，例如 Claude、Cursor 或 Grok/Grok Bot。

OpenAI dots 与 ChatGPT 的自定义 MCP 配置是两回事。dots 使用已安装的插件，而 KuroKit 尚未发布为插件。参见[客户端支持矩阵](client-support.md)。

## 故障排查

- 没有 KuroKit 工具或返回 401：创建一个新 token，并粘贴到 MCP 设置中。
- 提示 “Not in the MCP catalog”：选择自定义或手动远程 server 选项。
- 不要单独打开 `https://kurokit.com/oauth/authorize`。只有当应用在地址栏带上 `?client_id=` 打开它时才有效。
- 更多帮助：[故障排查](troubleshooting.md)。
