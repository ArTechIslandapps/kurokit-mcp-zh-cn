# 故障排查

如果本页无法解决问题，请先查看官方指南：https://kurokit.com/docs/mcp

## 应用里没有 KuroKit 工具，或返回 401

1. 打开 https://kurokit.com/dashboard/mcp
2. 创建一个新 token。
3. 把它粘贴到应用的 MCP 设置中。
4. 重新加载 MCP，或重启应用。
5. 再次询问：`Call the KuroKit MCP tool listMonitors.`

不要把 token 粘贴到聊天框里。

如果你是从应用商店添加的 KuroKit，请先移除再重新添加，然后登录。

## 我的应用是 Meta Muse 或 OpenAI dots

这些应用没有公开通用的自定义 MCP 网址流程。Muse 的连接器需经过 Muse Connector Platform，dots 则使用已安装的插件。目前这两个目录都未收录 KuroKit。请改用 Claude、Cursor、ChatGPT/Codex、Grok/Grok Bot 或 KuroKit 控制台。参见[客户端支持矩阵](client-support.md)。

## 应用提示 “not in the MCP catalog”

选择 **Add custom** 或 **manual remote server**，然后使用：

```text
https://mcp.kurokit.com/mcp
```

请使用小写形式。不要在末尾添加斜杠，也不要在 `/mcp` 后添加路径。

## 应用要求提供 token 或使用 OAuth

- 如果应用打开了一个地址栏带有 `?client_id=` 的登录页面，请在该页面完成授权。你可能不需要 token。
- 如果应用要求提供 token，请在 https://kurokit.com/dashboard/mcp 创建一个，并粘贴到应用的 MCP 设置中。
- 不要单独打开 `https://kurokit.com/oauth/authorize`。没有 MCP 应用附加的 `?client_id=` 值，它不会起任何作用。

## 网页监控项的 Last check 一直为空

负责打开页面的是浏览器扩展（Chrome 或 Edge）。请在被监控的页面上检查：

1. 保持被监控的页面打开。
2. 在该标签页上点击 KuroKit 图标，而不是在控制台标签页上。
3. 确认 **This site** 对被监控站点显示 **Allowed**。
4. 如果看到 **Allow** 按钮，请点击它，并在浏览器弹窗中确认。
5. 保持浏览器打开。关闭浏览器或让电脑休眠会中断检查。

**Connected** 只表示扩展已登录，并不表示站点已被允许。

## 首次检查显示 0 个事件

这是正常的。首次检查只保存快照，第二次检查才可能发现变化。等待 5–10 分钟，然后再次查询事件。

## 监控项数量已达上限

免费方案包含一个生效中的监控项。请删除旧监控项或将其移到 Saved 暂存区，然后重试。当前限制见 https://kurokit.com/pricing。

## 页面需要登录

当已安装浏览器扩展（Chrome 或 Edge）且站点已被允许时，KuroKit 可以监控你已经登录的页面。AI 应用不会收到你的密码或会话 cookie。

## 页面使用 JavaScript 构建

请让助手使用浏览器标签页渲染模式，并在浏览器中保持该页面打开。浏览器必须保持运行。

## 助手找不到页面中正确的区域

请使用 KuroKit 控制台上的点击选取工具。如果监控项始终为空，请在那里设置监控区域。

## n8n、Make、Zapier 或我的 webhook 收不到任何内容

1. 编辑监控项，确认已勾选 **Webhook** 且网址已保存。
2. 使用工作流的生产环境网址，而不是只在短时间内监听的测试网址。
3. 启用工作流。
4. 在 5 秒内返回 `2xx` 响应。需要长时间处理的工作流应先确认收到。
5. 确认监控项确实匹配到了变更。首次检查只会创建快照。

KuroKit 会对失败的投递进行重试，但如果接收方始终不返回 `2xx`，最终将不再收到重试。请查看该监控项最近一次的 webhook 错误。参见 [Webhook 与工作流自动化](webhooks.md)。

## 我的 webhook 网址被拒绝

该网址必须是公开的 `https://` 地址。localhost 和私有网络地址会被拒绝。接收方必须能从公网访问。

## 我的 webhook 测试能收到，但真实变更收不到

- 确认工作流处于启用状态。
- 确认监控项已匹配到变更。
- 确认你的筛选条件没有强制要求 `test: true`；真实投递不包含该字段。
- 使用 `event_id` 对重试进行去重。

## 我只需要让 AI 助手读取变更

不要使用 webhook。让监控项保持仅拉取，并用 `since=` 调用 MCP 的 `getEvents`。无需选择通道。

## 仍然无法解决

- 常见问题：https://kurokit.com/docs/faq
- 联系方式：support@kurokit.com
- 请提供监控项名称、页面网址和错误信息。不要发送你的 token。
