# 将 KuroKit 连接到 Grok 或 Grok Bot

Grok 和 Grok Bot 都支持自定义远程 MCP server。KuroKit 不在 Grok 的连接器目录中，因此需作为自定义 server 添加。

Grok Bot 是 Cursor 的云端 agent，与 grok.com 上的 Grok 并非同一产品，两者的配置界面也不同。

官方且持续更新的 KuroKit 指南：https://kurokit.com/docs/mcp

## grok.com 上的 Grok

1. 打开 https://grok.com/connectors。
2. 选择 **New Connector**，然后选择 **Custom**。
3. 输入 KuroKit 的 MCP 网址：

   ```text
   https://mcp.kurokit.com/mcp
   ```

4. 完成身份验证步骤。如果 Grok 打开了 KuroKit 登录页面，优先使用 OAuth。如果 Grok 只提供 token 输入框，请在 https://kurokit.com/dashboard/mcp 创建 token，并填入该输入框，而不是聊天框中。
5. 让 Grok 调用 `listMonitors` 以确认连接。

Grok 要求 server 可通过公网 HTTPS 访问。KuroKit 的托管端点已满足该要求。

Grok 官方文档：https://docs.x.ai/grok/connectors

## Grok Bot

Grok Bot 通过插件支持自定义远程 MCP server。该 server 必须可通过公网 HTTPS 访问，并且支持 OAuth 登录。具体添加流程可能因 Grok Bot 版本而异。

1. 安装 Grok Bot 并登录。Grok Bot 需要符合条件的付费 Cursor 方案，或已关联的 SuperGrok 订阅。
2. 打开侧边栏中的 **Connect apps**。如果 Grok Bot 提供自定义 MCP 或自定义插件选项，请选择它。
3. 输入 `https://mcp.kurokit.com/mcp`。
4. 如果出现提示，请完成 KuroKit 的 OAuth 登录。如果应用另外提供凭据输入项，请使用来自 https://kurokit.com/dashboard/mcp 的 token，并避免它出现在聊天中。
5. 如果你属于某个 Cursor 团队，管理员可能需要在团队的插件或 MCP 策略中允许该连接器。
6. 让 Bot 调用 `listMonitors` 以确认连接。

某些 Grok Bot 版本通过聊天让 Bot 添加自定义 MCP server。如果你的版本没有表单，请在聊天中让它添加 KuroKit MCP server，并按提示操作。不要使用 localhost 网址；Grok Bot 运行在云端。

Grok Bot 官方文档：

- https://docs.x.ai/grok-bot
- https://docs.x.ai/grok-bot/team-bots
- https://cursor.com/help/grok-bot/connect-plugins

## 身份验证说明

- 优先使用 OAuth。必须由 MCP 应用打开登录页面；不要单独打开 `https://kurokit.com/oauth/authorize`。
- 如果无法使用 OAuth 且应用要求提供 token，请在 https://kurokit.com/dashboard/mcp 创建一个，并粘贴到应用的设置中，而不是聊天框里。
- Grok Bot 团队管理员可以限制连接器。如果 KuroKit 显示为被管理员禁用，请联系管理员允许它。

## 检查连接

打开一个新对话并询问：

```text
Call the KuroKit MCP tool listMonitors. Do not fetch any web pages.
Tell me how many monitors I have and their names.
```

如果助手列出了你的监控项，说明连接正常。

## 如果无法正常工作

- 没有 KuroKit 工具或返回 401：使用 OAuth 重新连接，或创建新 token 并更新应用设置。
- 提示 “Not in the catalog”：选择自定义或手动远程 server 选项。
- Grok Bot 无法访问该网址：确认它是公网 HTTPS 地址，且以 `/mcp` 结尾。
- 更多帮助：[故障排查](troubleshooting.md) 和 https://kurokit.com/docs/mcp。
