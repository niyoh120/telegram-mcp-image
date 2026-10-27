# telegram-mcp-image

跟随 [chigwell/telegram-mcp](https://github.com/chigwell/telegram-mcp) 上游 commit 自动构建并推送 GHCR 镜像。

上游不发布预构建镜像（GHCR 403 / Docker Hub 无包），所以这里按 peach-intel 的模式自建：每日解析上游 `main` 的 commit SHA，用该 commit 的 Dockerfile 构建 `linux/amd64` 镜像，打 `sha-<12>` 与 `latest` 两个 tag；同一 commit 已构建过就跳过。

## 镜像

```
ghcr.io/niyoh120/telegram-mcp:latest
ghcr.io/niyoh120/telegram-mcp:sha-<12位commit>
```

## 部署

Coolify 上以 `dockerimage` pack 引用，端口 8765，HTTP transport：

```env
TELEGRAM_API_ID=2040
TELEGRAM_API_HASH=<Telegram Desktop 公开 hash>
TELEGRAM_SESSION_STRING=<账号 session string>
TELEGRAM_EXPOSED_TOOLS=all
MCP_TRANSPORT=http
MCP_HOST=0.0.0.0
MCP_PORT=8765
MCP_ALLOWED_HOSTS=telegram-mcp.niyoh.top,127.0.0.1,localhost
```

## 手动触发

```bash
gh workflow run build -R niyoh120/telegram-mcp-image
```

## 固定版本

想锁定某次构建，Coolify 里把 tag 从 `latest` 换成 `sha-<commit>`；回滚同理。
