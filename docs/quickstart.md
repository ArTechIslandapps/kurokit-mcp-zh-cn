# 快速开始

本指南介绍如何把 KuroKit 连接到 Claude、Cursor、ChatGPT/Codex、Grok/Grok Bot 或其他支持 MCP 的应用，整个过程大约需要 10 分钟。

你不需要下载 MCP server。KuroKit 已为你托管：`https://mcp.kurokit.com/mcp`。

## 准备工作

- 一个 KuroKit 账户：https://kurokit.com/login
- 一个支持 MCP 的 AI 应用
- KuroKit 浏览器扩展（Chrome 或 Edge），仅在需要监控网页时才需要

RSS 和官方 API 监控不需要扩展。

## 1. 创建账户

在 https://kurokit.com/login 注册并保持登录状态。

免费方案包含一个生效中的监控项，且不会过期。当前方案限制见 https://kurokit.com/pricing。

## 2. 准备扩展和 token

### 浏览器扩展（Chrome 或 Edge）

如果你只需要 RSS 或官方 API 监控，可以跳过本节。

1. 安装 KuroKit 浏览器扩展：
   - Chrome：https://chromewebstore.google.com/detail/kurokit-webpage-change-mo/fndfgkdjhbaoiadepfhgmhbgfjgfpiao
   - Edge：https://microsoftedge.microsoft.com/addons/detail/kurokit-webpage-change-/oiohkchggekefhcdamceinpmabpfiifk
2. 打开扩展并点击 Connect。
3. 登录 KuroKit 并授权该扩展。
4. 扩展弹窗应显示 Connected。

扩展负责为 KuroKit 打开网页。MCP 本身无法使用你的浏览器会话。

### Token

1. 打开 https://kurokit.com/dashboard/mcp
2. 点击 **Create token**。
3. 复制该 token。

把 token 粘贴到 AI 应用的 MCP 设置中，而不是聊天框里。如果你的应用使用 OAuth 并会打开登录页面，你可能不需要 token。

## 3. 把 KuroKit 添加到 AI 应用

### 如果应用的连接器商店中有 KuroKit

1. 打开应用的连接器商店或 MCP 应用商店。
2. 搜索 KuroKit。
3. 添加并完成登录。

有该条目时，优先使用这种方式。

### 如果 KuroKit 未被收录

添加自定义远程 MCP server：

- 网址：`https://mcp.kurokit.com/mcp`
- 请求头：`Authorization: Bearer <your token>`

有些应用把这一操作称为 “Add custom”、“Add remote server” 或 “Manual configuration”。如果应用要求提供配置文件，请使用 [examples](../examples/) 中的 JSON 或 YAML 示例。

Grok 和 Grok Bot 支持自定义远程 MCP server，参见 [Grok / Grok Bot](connect-grok-bot.md)。Meta Muse 和 OpenAI dots 使用各自的连接器或插件目录，目前尚未收录 KuroKit。请查看[客户端支持矩阵](client-support.md)。

不要单独打开 `https://kurokit.com/oauth/authorize`。只有当 MCP 应用在地址栏带上 `?client_id=` 打开它时才有效。

## 4. 检查连接

重新加载应用的 MCP 连接，或重启应用。打开一个新对话并粘贴：

```text
Call the KuroKit MCP tool listMonitors. Do not fetch any web pages.
Tell me how many monitors I have and their names.
```

如果助手列出了你的监控项，说明连接正常。列表为空也没有关系。

如果出现身份验证错误，请创建一个新 token 并粘贴到应用的设置中，不要粘贴到聊天框里。

## 5. 运行 Wikipedia 测试

这一步检查完整链路：AI 应用 → KuroKit → 浏览器扩展 → 网页。

1. 让助手为 `https://en.wikipedia.org/wiki/Special:RecentChanges` 创建一个监控项。
2. 在浏览器中打开返回的 `targetUrl`。
3. 在 Wikipedia 标签页上点击 KuroKit 图标。
4. 如果面板显示 **Allow en.wikipedia.org**，请点击它。“This site” 必须显示 **Allowed**。
5. 保持浏览器打开，等待 5–10 分钟。
6. 让助手再次检查是否有变更。

首次检查只保存快照，第二次检查才可能发现变化。

## 6. 监控你自己的页面

让助手为你的网页、feed 或 API 创建一个监控项。请明确说明什么情况算作变更。

例如：

```text
Watch this page: https://example.com/pricing
Tell me when the monthly price changes.
Create a KuroKit webpage monitor.
I will allow this site in the browser extension and keep the browser open.
```

如果助手无法选到页面中正确的区域，请使用 KuroKit 控制台上的点击选取工具。

## 7. 把变更推送到工作流（可选）

如果你使用 n8n、Make、Zapier 或自己的服务，请编辑监控项，勾选 **Webhook**，然后粘贴工作流的公开 HTTPS 网址。KuroKit 会 POST 带签名的 JSON，并对失败的投递进行重试。参见 [Webhook 与工作流自动化](webhooks.md)。

MCP 的 `getEvents` 只支持拉取（pull），无需选择通道。

## 下一步

- [连接任意 MCP 客户端](connect-any-client.md)
- [Webhook 与工作流自动化](webhooks.md)
- [客户端支持矩阵](client-support.md)
- [故障排查](troubleshooting.md)
- [常见问题](faq.md)
- 官方指南：https://kurokit.com/docs/mcp
