# 连接任意 MCP 客户端

如果你的应用不是 Claude、Cursor、ChatGPT/Codex 或 Grok/Grok Bot，或者应用的连接器商店中没有 KuroKit，请使用本页。对于 Meta Muse 和 OpenAI dots，请先查看[客户端支持矩阵](client-support.md)；这些应用使用各自的连接器或插件目录。

## 1. 先查看应用的商店

打开应用的连接器商店或 MCP 应用商店，搜索 KuroKit。如果找到了，直接在那里添加并完成登录。这通常是最简单的方式。

## 2. 添加自定义远程 server

如果 KuroKit 未被收录，请添加远程 MCP server：

| 字段 | 值 |
| --- | --- |
| 名称 | KuroKit |
| 网址 | `https://mcp.kurokit.com/mcp` |
| 请求头 | `Authorization: Bearer <your token>` |

在 https://kurokit.com/dashboard/mcp 创建 token。

有些应用使用 JSON 或 YAML 配置文件而不是表单。参见 [examples](../examples/)。

## 3. 测试连接

重新加载应用的 MCP 连接，或重启应用。打开一个新对话并询问：

```text
Call the KuroKit MCP tool listMonitors. Do not fetch any web pages.
Tell me how many monitors I have and their names.
```

如果助手列出了监控项，说明连接正常。

## 身份验证说明

- 如果应用支持 OAuth，并打开了一个地址栏带有 `?client_id=` 的登录页面，请在该页面完成授权。你可能不需要 token。
- 如果应用要求提供 token，请把它粘贴到应用的 MCP 设置中，不要粘贴到聊天框里。
- 不要单独打开 `https://kurokit.com/oauth/authorize`。没有 MCP 应用附加的 `?client_id=` 值，它不会起任何作用。
- 如果应用提示 “not in the MCP catalog”，请选择 “Add custom” 或 “manual remote server”。

## 下一步

- [快速开始](quickstart.md)
- [客户端支持矩阵](client-support.md)
- [Grok / Grok Bot](connect-grok-bot.md)
- [故障排查](troubleshooting.md)
- [示例配置](../examples/)
