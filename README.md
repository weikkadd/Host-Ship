# Host-Ship Auto Renew

Host-Ship 免费服务器自动续期脚本（GitHub Actions 版）。https://panel.host-ship.com/

## 功能

- 每天北京时间 08:00 自动检查，仅在出现可用 `Renew` 按钮时续期
- 未到续期窗口（`Renew Limit Reached`）不会点击续期
- Telegram 通知：检查结果 / 续期成功 / 失败异常
- 支持可选 `NODE_LINK` 固定出口 IP（VMess / VLESS）

## GitHub Secrets

`Settings -> Secrets and variables -> Actions` 添加：

| Name | 说明 |
|------|------|
| `SERVER_URL` | 服务器详情页地址，如 `https://panel.host-ship.com/server/xxxxxxxx`（必填） |
| `HOSTSHIP_COOKIE` | 浏览器登录后的 Cookie 字符串（必填，绕过 Turnstile） |
| `HOSTSHIP_LOGIN` | 登录邮箱（Cookie 失效时备用） |
| `HOSTSHIP_PASSWORD` | 登录密码（Cookie 失效时备用） |
| `TG_BOT_TOKEN` | Telegram Bot Token（必填） |
| `TG_CHAT_ID` | 接收通知的 Chat ID（必填） |
| `NODE_LINK` | `vmess://...` / `vless://...` 分享链接（可选） |

## 导出 Cookie

Host-Ship 登录页有 Cloudflare Turnstile 人机验证，自动登录会被拦截。需在本地浏览器手动登录后导出 Cookie：

1. 用 Chrome / Edge 打开 `panel.host-ship.com` 并登录
2. 按 `F12` 打开开发者工具 → **Network** 标签
3. 刷新页面，点任意一个请求 → **Headers** → 找 `Request Headers` 里的 `Cookie:` 行
4. 复制整行 Cookie 值（一长串 `key1=val1; key2=val2; ...`）
5. 存到 GitHub Secrets `HOSTSHIP_COOKIE`

Cookie 过期后脚本会 TG 通知，重复上述步骤更新即可。

## 使用

- **手动测试**：`Actions -> Host-Ship Auto Renew -> Run workflow`，手动运行会发送一条 Telegram 检查消息
- **自动运行**：每天北京时间 08:00 自动检查续期

## 说明

- 所有敏感信息只存放在 Repository secrets，不要写入代码或 README
- Actions 定时任务可能有几分钟延迟，属正常现象
- Host-Ship 页面结构变化后可能需要更新脚本选择器
