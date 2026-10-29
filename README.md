# KuroKit MCP

KuroKit 监控网页、RSS 源和官方 API。当有内容发生变化时，它会通知你的 AI 助手。

本仓库是托管版 KuroKit MCP server 的安装指南和示例中心。你无需下载或自行运行 server。

- 官方安装指南：https://kurokit.com/docs/mcp
- 控制台：https://kurokit.com
- 价格：https://kurokit.com/pricing
- Chrome 扩展：https://chromewebstore.google.com/detail/kurokit-webpage-change-mo/fndfgkdjhbaoiadepfhgmhbgfjgfpiao
- Edge 扩展：https://microsoftedge.microsoft.com/addons/detail/kurokit-webpage-change-/oiohkchggekefhcdamceinpmabpfiifk
- MCP 端点：https://mcp.kurokit.com/mcp

## 快速开始

1. 创建一个免费的 KuroKit 账户，并保持登录状态。
2. 如需监控网页，请安装浏览器扩展（Chrome 或 Edge）。RSS 和官方 API 监控不需要扩展。
3. 把 KuroKit 添加到你的 AI 应用。如果应用自带的连接器商店里有 KuroKit，直接使用该条目并登录；否则把它添加为自定义远程 MCP server。
4. 让你的助手列出你的监控项。如果它能列出来，说明连接正常。
5. 为你自己的网页、feed 或 API 创建一个监控。

完整操作步骤（包括 Wikipedia 测试）见 https://kurokit.com/docs/mcp。

## 连接你的 AI 应用

优先使用商店中的条目：

1. 打开你的 AI 应用的连接器商店或 MCP 应用商店。
2. 搜索 KuroKit。
3. 添加并完成登录。

如果 KuroKit 未被收录，请添加自定义远程 MCP server：

- 网址：`https://mcp.kurokit.com/mcp`
- 身份验证：`Authorization: Bearer <your KuroKit token>`

在 https://kurokit.com/dashboard/mcp 创建 token。请把它粘贴到应用的 MCP 设置中，而不是聊天框里。

大多数应用只需要 token 或 OAuth 登录中的一种，不需要两者都配。如果应用打开了一个地址栏带有 `?client_id=` 的登录页面，请在该页面完成授权。不要单独打开 `https://kurokit.com/oauth/authorize`；只有 MCP 应用打开它时才有效。

各客户端说明：

- [Claude](docs/connect-claude.md)
- [Cursor](docs/connect-cursor.md)
- [ChatGPT / Codex](docs/connect-chatgpt.md)
- [Grok / Grok Bot](docs/connect-grok-bot.md)
- [其他 MCP 客户端](docs/connect-any-client.md)

Meta Muse 和 OpenAI dots 使用各自的连接器或插件目录，目前尚未收录 KuroKit。在向用户承诺这些应用的配置方式之前，请先查看[客户端支持矩阵](docs/client-support.md)。

## 把变更推送到工作流

如果你希望 n8n、Make、Zapier 或你自己的服务立即响应，请使用 webhook。编辑监控项，勾选 **Webhook**，然后粘贴工作流的公开 HTTPS 网址。KuroKit 会 POST 带签名的 JSON，并对失败的投递进行重试。参见 [Webhook 与工作流自动化](docs/webhooks.md)。

MCP 的 `getEvents` 只支持拉取（pull），无需选择通道。

## 可以监控哪些内容

- 通过浏览器扩展（Chrome 或 Edge）监控**网页变化**。这既适用于公开页面，也适用于你已经登录的页面。
- 无需扩展即可监控 **RSS 源**。
- 无需扩展即可监控**官方 API**。

免费方案包含一个生效中的监控项。当前限制见 https://kurokit.com/pricing。

## 示例

[examples](examples/) 文件夹中提供了备用的自定义 server 配置：

- [带 token 的 MCP JSON](examples/mcp.json)
- [仅使用 OAuth 的 MCP JSON](examples/mcp-oauth.json)
- [带 token 的 Hermes YAML](examples/hermes.yaml)
- [使用 OAuth 的 Hermes YAML](examples/hermes-oauth.yaml)

保存前请替换 `PASTE_TOKEN_FROM_DASHBOARD_MCP`。切勿提交真实 token。

## 常见问题

**应用里没有 KuroKit 工具，或返回 401。**
在 https://kurokit.com/dashboard/mcp 创建一个新 token，粘贴到应用的设置中，然后重新加载应用。

**应用提示 “not in the MCP catalog”。**
选择 “Add custom” 或 “manual remote server”，然后以小写形式填写 `https://mcp.kurokit.com/mcp`。

**监控项的 Last check 一直为空。**
打开被监控的页面，在该标签页上点击 KuroKit 扩展图标，并确认 “This site” 显示 Allowed。浏览器必须保持打开。

**首次检查没有任何事件。**
这是正常的。首次检查只保存快照，第二次检查才可能发现变化。

**n8n、Make 或 Zapier 收不到任何内容。**
在监控项中勾选 **Webhook**，使用工作流的生产环境 HTTPS 网址，启用工作流，并尽快返回 `2xx` 响应。参见 [Webhook 与工作流自动化](docs/webhooks.md)。

更多帮助：[故障排查](docs/troubleshooting.md) 和 https://kurokit.com/docs/faq。

## 安全

- 不要把密码、token 或卡号粘贴到 AI 对话中。
- 在 https://kurokit.com/dashboard/mcp 创建和撤销 token。
- 仅在 KuroKit 或 Stripe 页面上确认付款。
- 漏洞报告方式见 [SECURITY.md](SECURITY.md)。

## 文档

- [快速开始](docs/quickstart.md)
- [客户端支持矩阵](docs/client-support.md)
- [连接任意 MCP 客户端](docs/connect-any-client.md)
- [Webhook 与工作流自动化](docs/webhooks.md)
- [故障排查](docs/troubleshooting.md)
- [常见问题](docs/faq.md)
- [截图清单](assets/README.md)
- [供 AI 读取的摘要](llms.txt)

## 许可

文档和图片采用 [CC BY 4.0](LICENSE) 许可。JSON、YAML 及其他代码示例采用 [MIT License](LICENSE-CODE) 许可。

## 关于本仓库

本仓库是从 KuroKit 私有仓库生成的纯文档镜像。MCP server、Web 应用和扩展的源代码不在此发布。直接在本仓库所做的修改可能会被下次同步覆盖。
