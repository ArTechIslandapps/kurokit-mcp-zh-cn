# 将 KuroKit 连接到 Cursor

不同 Cursor 版本的设置界面可能有所不同。官方且持续更新的指南见 https://kurokit.com/docs/mcp。

## 如果 Cursor 的 MCP 应用商店中有 KuroKit

1. 打开 Cursor 的 MCP 设置或应用商店。
2. 搜索 KuroKit。
3. 添加并完成登录。

有该条目时优先使用这种方式。你无需粘贴 token。

## 如果 KuroKit 未被收录

在 Cursor 的 MCP 设置中添加自定义远程 MCP server：

- 网址：`https://mcp.kurokit.com/mcp`
- 请求头：`Authorization: Bearer <your token>`

或者把以下内容粘贴到 MCP 配置文件中：

```json
{
  "mcpServers": {
    "kurokit": {
      "url": "https://mcp.kurokit.com/mcp",
      "headers": {
        "Authorization": "Bearer PASTE_TOKEN_FROM_DASHBOARD_MCP"
      }
    }
  }
}
```

在 https://kurokit.com/dashboard/mcp 创建 token。保存前请替换 `PASTE_TOKEN_FROM_DASHBOARD_MCP`。不要把 token 粘贴到聊天框里。

同样的示例也可在 [examples/mcp.json](../examples/mcp.json) 中找到。

## 检查连接

打开一个新对话并询问：

```text
Call the KuroKit MCP tool listMonitors. Do not fetch any web pages.
Tell me how many monitors I have and their names.
```

如果 Cursor 列出了你的监控项，说明连接正常。

## 故障排查

- 没有 KuroKit 工具或返回 401：创建一个新 token，并粘贴到 MCP 配置中。
- 如果配置中已有其他 server，只需添加 `kurokit` 块，不要替换整个文件。
- 不要单独打开 `https://kurokit.com/oauth/authorize`。只有当 Cursor 在地址栏带上 `?client_id=` 打开它时才有效。
- 更多帮助：[故障排查](troubleshooting.md)。
