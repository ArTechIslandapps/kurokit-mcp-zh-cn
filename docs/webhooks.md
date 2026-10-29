# Webhook 与工作流自动化

如果你希望 KuroKit 把变更推送到 n8n、Make、Zapier 或你自己的服务，请使用 webhook。

这与 MCP 不同。`getEvents` 是拉取模式：由你的 AI 应用向 KuroKit 请求事件。webhook 是推送模式：当监控项匹配成功时，KuroKit 向你的网址发送一个 HTTP `POST` 请求。

你可以在同一个监控项上同时使用两者。

## 拉取还是推送？

| 方式 | 方向 | 适用场景 |
| --- | --- | --- |
| MCP `getEvents` | 拉取 | 需要在被询问时或按计划检查变更的 AI 助手。无需为每个监控项配置通道。 |
| Webhook | 推送 | 需要立即响应的 n8n、Make、Zapier 或你自己的后端。 |
| Email / Telegram | 推送 | 希望在邮箱或聊天中收到提醒的个人用户。 |

MCP 没有监控项匹配的实时事件流，也没有 `registerTrigger` 工具。请使用 `since=` 轮询 `getEvents` 以进行增量读取。

## 开始之前

你需要：

- 一个 KuroKit 账户和一个监控项。
- 一个公开的 `https://` 接收网址。
- 一个能在 5 秒内返回 `2xx` 响应的接收方。

localhost 和私有网络网址会被拒绝。接收方必须能从公网访问。

## 在控制台中启用 webhook

1. 打开监控项并编辑它。
2. 在 **Notify me via** 下勾选 **Webhook**。
3. 粘贴工作流中 webhook 节点提供的网址。
4. 保存监控项。
5. 签名密钥出现时立即复制。它只显示一次。
6. 从监控项发送一条测试通知，确认接收方能够收到。

测试投递的 JSON 正文中包含 `"test": true`。请使用该字段避免把测试当真实变更处理。

之后修改网址会保留同一个签名密钥。移除 webhook 通道后即停止投递。

## 通过 MCP 创建 webhook 监控项

当 AI 助手创建或更新监控项时，在 `notifyChannels` 中加入 `webhook`，并提供 `webhookUrl`：

```json
{
  "notifyChannels": ["webhook"],
  "webhookUrl": "https://your-receiver.example.com/kurokit"
}
```

写入响应中包含一次性的 `webhookSecret`。请把它保存到接收方的环境变量中。`listMonitors` 会显示最近的 webhook 错误，但绝不会显示密钥。

## 请求格式

KuroKit 发送：

```text
POST /your-path HTTP/1.1
Content-Type: application/json
X-KuroKit-Event: monitor.change
X-KuroKit-Signature: t=1760000000,v1=<hex hmac>
```

请求正文示例：

```json
{
  "schema_version": 2,
  "monitor_id": "11111111-1111-1111-1111-111111111111",
  "event_id": "22222222-2222-2222-2222-222222222222",
  "monitor_name": "Competitor pricing",
  "subject": "KuroKit: Competitor pricing",
  "body": "The watched text changed.",
  "target_url": "https://example.com/pricing",
  "diff_summary": "The watched text changed.",
  "judged_by": "rule",
  "added": ["Pro plan: $49"],
  "removed": ["Pro plan: $39"],
  "sent_at": "2026-10-21T12:00:00.000Z"
}
```

当字段不可用时，`event_id`、`monitor_name`、`target_url`、`diff_summary`、`added` 和 `removed` 等字段会被省略。`test: true` 仅出现在测试投递中。

如果你的工作流可能收到重复投递，请使用 `event_id` 作为幂等键。

## 验证签名

签名是对时间戳、一个英文句点以及原始请求正文计算的 HMAC-SHA256：

```text
signature = HMAC-SHA256(secret, "<t>.<raw body>")
```

`X-KuroKit-Signature` 请求头包含 `t=<unix seconds>` 和 `v1=<hex digest>`。请在解析 JSON 之前用原始正文进行验证，并拒绝过期的时间戳以降低重放风险。

Node.js 示例：

```js
import crypto from "node:crypto";

function verifyKuroKitSignature(rawBody, header, secret, maxAgeSeconds = 300) {
  const parts = Object.fromEntries(
    header.split(",").map((part) => {
      const index = part.indexOf("=");
      return [part.slice(0, index), part.slice(index + 1)];
    })
  );

  const timestamp = Number(parts.t);
  const received = parts.v1;
  if (!Number.isFinite(timestamp) || !received) return false;
  if (Math.abs(Date.now() / 1000 - timestamp) > maxAgeSeconds) return false;

  const expected = crypto
    .createHmac("sha256", secret)
    .update(`${timestamp}.${rawBody}`)
    .digest("hex");

  const expectedBuffer = Buffer.from(expected, "hex");
  const receivedBuffer = Buffer.from(received, "hex");
  return (
    expectedBuffer.length === receivedBuffer.length &&
    crypto.timingSafeEqual(expectedBuffer, receivedBuffer)
  );
}
```

密钥应保存在服务端。不要把它放进浏览器、公开的工作流或 AI 对话中。

## n8n

1. 添加一个 **Webhook** 节点。
2. 将 **HTTP Method** 设为 `POST`。
3. 将 **Respond** 设为 `Immediately`，响应码设为 `200`。
4. 复制 **Production URL**。测试网址仅在工作流正在监听测试事件时有效。
5. 把生产环境网址粘贴到 KuroKit 监控项的 **Webhook URL** 字段并保存。
6. 把签名密钥复制到 n8n 的凭据或环境变量中。
7. 启用工作流。
8. 发送一条 KuroKit 测试通知。如果不希望测试事件触发真实操作，请按 `test: true` 进行筛选或分支。

如果你的 n8n 版本提供 raw-body 选项，请在验证签名时启用它。否则请在小型代理中验证签名，或跳过签名验证并将 webhook 网址当作密钥保护。

请尽快返回 `2xx`。如果工作流需要长时间处理，请先确认收到，再异步继续；KuroKit 会在 5 秒后超时。

## Make 与 Zapier

- **Make：** 添加 **Custom webhook**，复制其地址，并粘贴到 KuroKit。
- **Zapier：** 使用 **Webhooks by Zapier -> Catch Hook**，复制网址，并粘贴到 KuroKit。

两者都应立即返回成功响应。如果工作流可能收到重试，请使用 `event_id` 去重。

## 重试与错误

- 超时：每次尝试 5 秒。
- 重试：约在 1 分钟、5 分钟、30 分钟和 30 分钟后各一次。
- 最多尝试：5 次（含首次）。
- 超过后，KuroKit 将停止重试该次投递。

监控项的编辑页面会显示最近的 webhook 错误。MCP 的 `listMonitors` 也会返回 `webhook.lastError`。投递失败并不意味着变更丢失；它仍在事件日志中，可通过 `getEvents` 读取。

## 故障排查

**工作流收不到任何内容**

- 确认已勾选 **Webhook** 且网址已保存。
- 使用生产环境网址，而不是 n8n 的测试网址。
- 启用 n8n 工作流。
- 确认接收方在 5 秒内返回 `2xx`。
- 确认监控项已匹配到变更。首次检查只会创建快照。

**KuroKit 拒绝该网址**

该网址必须是公开的 `https://` 地址。localhost、私有 IP 和非 HTTPS 网址都会被拒绝。

**测试能收到，但真实变更收不到**

- 确认工作流处于启用状态。
- 确认监控项确实匹配成功。
- 确认你没有因为要求 `test: true` 而把所有真实事件都过滤掉。

**接收方收到重复请求**

重试和至少一次（at-least-once）投递可能产生重复。请使用 `event_id` 去重。

**我只需要让 AI 助手读取变更**

不要使用 webhook。让监控项保持仅拉取，并用 `since=` 调用 MCP 的 `getEvents`。

## 相关文档

- [快速开始](quickstart.md)
- [故障排查](troubleshooting.md)
- [常见问题](faq.md)
- [供 AI 读取的摘要](../llms.txt)
