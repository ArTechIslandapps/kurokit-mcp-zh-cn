# 截图清单

请把文件放在本文件夹中。使用 PNG 格式，裁剪到相关窗口，并移除 token、email 地址、监控项名称及其他个人数据。

## 必需

| 文件名 | 展示内容 | 不得出现 |
| --- | --- | --- |
| `extension-connected-allowed.png` | 在被监控的标签页上打开浏览器扩展弹窗。“Account” 显示 Connected，“This site” 对该站点显示 Allowed。 | email、token、无关的标签页 |
| `dashboard-create-token.png` | https://kurokit.com/dashboard/mcp，且 “Create token” 按钮可见。 | token 本身 |
| `connector-store-kurokit.png` | 某个 AI 应用的连接器商店或 MCP 应用商店中的 KuroKit 条目。 | 账户 email、账单信息 |
| `connection-success.png` | 添加连接之后的 AI 对话，其中列出了用户的监控项。 | 敏感情况下为私密的监控项名称或网址 |

如果 KuroKit 目前未出现在任何公开商店中，`connector-store-kurokit.png` 是可选文件。不要伪造或编辑商店条目。

## 可选

| 文件名 | 展示内容 |
| --- | --- |
| `wikipedia-target-url.png` | 通过返回的 `targetUrl` 打开的 Wikipedia 最近更改页面，且 KuroKit 扩展图标可见。 |
| `dashboard-monitor.png` | 控制台中一个带有最近 Last check 时间的监控项。请隐藏私密的监控项名称和网址。 |
| `troubleshooting-allow-site.png` | 扩展面板中针对 `en.wikipedia.org` 显示 “Allow” 按钮。 |

## 图片规范

- 格式：PNG。
- 宽度：1200–1600 px 即可。
- 大小：尽量让每个文件保持在约 500 KB 以内。
- 除非浏览器边框有助于说明点击位置，否则请裁剪掉。
- 文件名使用小写字母和连字符。
- 切勿包含真实 Bearer token、email 地址、密码或卡号。

## 提交图片

普通 PNG 不需要 Git LFS。请让每个文件保持在上方建议的大小范围内。

不要先推送包含密钥的图片再事后补救。即使文件被替换，它仍会留在 Git 历史中。

在文档中引用图片时请使用相对路径。`docs/` 中的页面使用 `../assets/extension-connected-allowed.png`。
