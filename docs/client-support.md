# 客户端支持

最近核对时间：2026-10-21。

KuroKit 提供一个托管的远程 MCP 端点，多个 AI 应用可以直接添加。有些应用只通过自家审核过的连接器或插件目录来开放 MCP。

| 客户端 | 使用 MCP 的方式 | KuroKit 状态 | 指南 |
| --- | --- | --- | --- |
| Claude | 连接器目录或自定义远程 MCP | 使用直接配置方式 | [Claude](connect-claude.md) |
| Cursor | MCP 应用商店或自定义远程 MCP | 使用直接配置方式 | [Cursor](connect-cursor.md) |
| ChatGPT / Codex | 连接器或自定义远程 MCP（在支持的情况下） | 使用直接配置方式 | [ChatGPT / Codex](connect-chatgpt.md) |
| Grok / Grok Bot | 自定义远程 MCP 连接器 | Grok 和 Grok Bot 均支持；KuroKit 不在 Grok 目录中，需作为自定义 server 添加 | [Grok / Grok Bot](connect-grok-bot.md) |
| Meta Muse | 通过 Muse Connector Platform 提供 MCP 连接器；审核通过的连接器会出现在 Settings > Connectors | KuroKit 不是经过审核的 Muse 连接器，也没有公开的自助式 MCP 网址流程 | 见下方 Muse 说明 |
| OpenAI dots | 为 ChatGPT 账户安装的插件；插件可以包含 MCP server | KuroKit 尚未发布为插件，dots 也没有公开的自定义 MCP 网址设置 | 见下方 dots 说明 |
| 其他 MCP 客户端 | 自定义远程 MCP | 使用该端点，并配合 OAuth 或 token | [任意 MCP 客户端](connect-any-client.md) |

## Meta Muse

Meta 为 Muse 提供了 MCP 连接器文档，但公开连接器需要经过 Muse Connector Platform 的审核流程。审核通过的连接器可在 Muse 的 **Settings > Connectors** 中找到。

当某项服务不在列表中时，Muse 也可以创建 Custom Connector。Meta 表示不会审核自定义连接器，而且该流程可能要求填写 API 信息，而不是简单的 MCP 网址。不要想当然地认为粘贴一个网址就够了，也不要把 KuroKit token 粘贴到 Muse 聊天中。

KuroKit 目前不是经过审核的 Muse 连接器。在 KuroKit 上架或 Meta 提供自助式自定义 MCP 流程之前，请使用其他客户端。

来源：

- https://platform.muse.ai/docs
- https://www.meta.com/help/artificial-intelligence/1687253048996149/

## OpenAI dots

OpenAI dots 使用为你的 ChatGPT 账户安装并启用的受支持插件。OpenAI 的插件文档允许插件包含 MCP server，但 dots 的文档并未提供直接填写自定义 MCP 网址的设置。

KuroKit 目前尚未发布为 ChatGPT 插件，因此没有可直接写进文档的 dots 配置方式。请使用带自定义 MCP 连接器的 ChatGPT/Codex，或上表中的其他客户端。如果 KuroKit 日后以插件形式提供，请为需要使用它的账户和 dot 启用该插件。

来源：

- https://learn.chatgpt.com/docs/dots
- https://developers.openai.com/plugins/concepts/mcp-server

## 核对说明

“支持 MCP”并不意味着 KuroKit 已收录在该应用的目录中。要进入目录，需要单独提交并通过审核。上表中的状态描述的是有文档支持的能力，而非经过实测的 KuroKit 收录情况。
