# 将 KuroKit 连接到 Claude

不同 Claude 应用和方案中的菜单名称可能有所不同。官方且持续更新的指南见 https://kurokit.com/docs/mcp。

## 如果 Claude 的连接器目录中有 KuroKit

1. 打开 Claude 的连接器或 MCP 设置。
2. 搜索 KuroKit。
3. 添加并完成登录。

有该条目时优先使用这种方式。你无需粘贴 token。

## 如果 KuroKit 未被收录

把 KuroKit 添加为自定义远程 MCP server：

- 网址：`https://mcp.kurokit.com/mcp`
- 请求头：`Authorization: Bearer <your token>`

在 https://kurokit.com/dashboard/mcp 创建 token。请把它粘贴到 Claude 的 MCP 设置中，而不是聊天框里。

如果 Claude 要求提供 JSON 配置文件，请使用 [examples/mcp.json](../examples/mcp.json) 并替换其中的占位符。

## 检查连接

打开一个新对话并询问：

```text
Call the KuroKit MCP tool listMonitors. Do not fetch any web pages.
Tell me how many monitors I have and their names.
```

如果 Claude 列出了你的监控项，说明连接正常。

## 故障排查

- 没有 KuroKit 工具或返回 401：创建一个新 token，并粘贴到 MCP 设置中。
- 提示 “Not in the MCP catalog”：选择自定义或手动远程 server 选项。
- 不要单独打开 `https://kurokit.com/oauth/authorize`。只有当 Claude 在地址栏带上 `?client_id=` 打开它时才有效。
- 更多帮助：[故障排查](troubleshooting.md)。
