# 常见问题

## KuroKit 是什么？

KuroKit 监控网页、RSS 源或官方 API，并在符合你规则的内容发生变化时通知你的 AI 助手。你也可以在控制台中接收 email 或 Telegram 通知。

## 我需要写代码吗？

不需要。用自然语言描述你想监控的内容即可。助手可以创建监控项，你也可以直接使用控制台。

## 我需要下载 MCP server 吗？

不需要。KuroKit 已托管在 `https://mcp.kurokit.com/mcp`。

## 哪些 AI 应用可以连接？

Claude、Cursor、ChatGPT/Codex 和 Grok/Grok Bot 可以使用直接 MCP 配置。Meta Muse 和 OpenAI dots 使用各自的连接器或插件目录，目前尚未收录 KuroKit。参见[客户端支持矩阵](client-support.md)。

## 我需要浏览器扩展吗？

只有监控网页时才需要。RSS 和官方 API 监控不需要。

扩展负责为 KuroKit 打开网页，也让 KuroKit 能够使用你已经登录的浏览器会话。

## 免费方案会过期吗？

不会。免费方案包含一个生效中的监控项。当前限制见 https://kurokit.com/pricing。

## 我可以监控需要登录的页面吗？

可以，前提是已安装浏览器扩展（Chrome 或 Edge）、站点已被允许，并且你已登录。KuroKit 使用你的浏览器会话；你的 AI 应用不会收到你的密码或 cookie。

## 我可以监控大量使用 JavaScript 的页面吗？

可以，只要该页面在你的浏览器中打开。请保持浏览器运行。

## 在哪里获取 token？

打开 https://kurokit.com/dashboard/mcp 并点击 **Create token**。把 token 粘贴到 AI 应用的 MCP 设置中，而不是聊天框里。

## 为什么单独打开 `https://kurokit.com/oauth/authorize` 不起作用？

该页面需要 MCP 应用附加的 `?client_id=` 值。手动打开时无法建立连接。请改用应用的连接器商店，或添加自定义远程 server。

## 我可以把变更发送到 n8n、Make 或 Zapier 吗？

可以。编辑监控项，勾选 **Webhook**，然后粘贴工作流的公开 HTTPS 网址。保存监控项后，把签名密钥复制到你的工作流中。KuroKit 会 POST 带签名的 JSON，并对失败的投递进行重试。参见 [Webhook 与工作流自动化](webhooks.md)。

MCP 的 `getEvents` 只支持拉取（pull），无需选择通道。

## 在哪里查看我的监控项？

打开 https://kurokit.com/dashboard。你也可以让助手调用 `listMonitors`。

## 在哪里可以获得帮助？

- 安装指南：https://kurokit.com/docs/mcp
- 故障排查：[故障排查](troubleshooting.md)
- 常见问题：https://kurokit.com/docs/faq
- Email：support@kurokit.com
