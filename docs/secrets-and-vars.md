# Vars / Secrets 约定（Cloudflare Dashboard）

## 1) Variables（公开非敏感）
在 Cloudflare Worker 的 Settings → Variables and Secrets 中配置：

- `NEXT_PUBLIC_APP_URL`
- `NEXT_PUBLIC_DEFAULT_LOCALE`
- `NEWS_API_BASE`
- `ANALYZER_BASE_URL`
- `GEMINI_MODEL`
- `GEMINI_API_BASE`

## 2) Secrets（仅服务端）
- `GEMINI_API_KEY`
- `ANALYZER_TOKEN`
- `AUTH_SECRET`

## 3) 与代码的映射关系
所有环境变量读取统一在 `apps/web/lib/env.ts`：

- `appEnv.public.*` 对应前端可见变量
- `appEnv.server.*` 对应服务端变量/密钥

不要在业务代码中散落读取 `process.env`。

## 4) 推荐配置策略
- Production 与 Preview 使用不同密钥（至少 `AUTH_SECRET` 分开）。
- `NEXT_PUBLIC_APP_URL`：
  - Production 指向正式域名（如 `https://your-domain.com`）
  - Preview 可指向 preview 域名
- `ANALYZER_BASE_URL` / `NEWS_API_BASE` 建议在 Preview 使用测试环境地址。

## 5) 最小可用示例

### Variables
- `NEXT_PUBLIC_APP_URL=https://your-domain.com`
- `NEXT_PUBLIC_DEFAULT_LOCALE=zh-CN`
- `NEWS_API_BASE=https://r.jina.ai/http://news.google.com/rss/search?q=golf`
- `ANALYZER_BASE_URL=https://your-analyzer.example.com`
- `GEMINI_MODEL=gemini-2.5-flash-lite`
- `GEMINI_API_BASE=https://generativelanguage.googleapis.com/v1beta`

### Secrets
- `GEMINI_API_KEY=***`
- `ANALYZER_TOKEN=***`
- `AUTH_SECRET=***`
