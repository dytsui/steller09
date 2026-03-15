# steller09 部署指南（GitHub + Cloudflare Git Integration）

> 目标：不依赖本机 `wrangler deploy`，通过 Cloudflare Dashboard 在线自动构建与部署。

## 1) 代码侧前置检查（仓库内）
在仓库根目录执行：

```bash
npm install
npm run typecheck:web
npm run build:web
npm run open-next:build
```

以上命令全部通过后，再接入 Cloudflare Git integration。

## 2) Cloudflare 资源准备
在 Cloudflare 账号中先创建并记录以下资源：

- D1 数据库：`steller09`
- R2 Bucket：
  - `steller09-videos`
  - `steller09-keyframes`
  - `steller09-shares`
  - `steller09-exports`

并将 D1 `database_id` 写入 `apps/web/wrangler.jsonc`（`d1_databases[0].database_id`）。

## 3) D1 初始化（仅一次）
可在 Cloudflare Dashboard D1 控制台执行 `cloudflare/schema.sql`，
或在 CI 环境中使用：

```bash
wrangler d1 execute steller09 --file cloudflare/schema.sql
```

## 4) 创建 Worker（Git 集成）
在 Cloudflare Dashboard：

1. Workers & Pages → **Create** → **Import a repository**。
2. 选择 GitHub 仓库 `steller09`。
3. 构建配置：
   - **Root directory**: `.`
   - **Build command**: `npm ci && npm run build:web && npm run open-next:build`
   - **Deploy command**: 留空（Workers Git 集成自动处理）
4. Cloudflare 将读取 `apps/web/wrangler.jsonc` 的入口与绑定（`main: .open-next/worker.js`、D1/R2 bindings 等）。

## 5) Dashboard 变量与密钥配置
在 Worker 项目设置中配置（Production / Preview 都建议分别配置）：

### Variables（非敏感）
- `NEXT_PUBLIC_APP_URL`
- `NEXT_PUBLIC_DEFAULT_LOCALE`
- `NEWS_API_BASE`
- `ANALYZER_BASE_URL`
- `GEMINI_MODEL`
- `GEMINI_API_BASE`

### Secrets（敏感）
- `GEMINI_API_KEY`
- `ANALYZER_TOKEN`
- `AUTH_SECRET`

## 6) Dashboard 绑定配置
在 Worker Settings → Bindings 中确认与 `apps/web/wrangler.jsonc` 一致：

- D1 binding: `DB` → `steller09`
- R2 bindings:
  - `VIDEOS` → `steller09-videos`
  - `KEYFRAMES` → `steller09-keyframes`
  - `SHARES` → `steller09-shares`
  - `EXPORTS` → `steller09-exports`

## 7) 自动部署流程
- 推送到默认分支：触发 Production 部署。
- 提交 PR：触发 Preview（若已开启 preview deployments）。

## 8) 验收清单（Done Definition）
- Next build 成功。
- OpenNext build 成功。
- 指定页面与 API 路由可访问。
- D1 schema 与代码字段一致。
- 不存在未解析 import/export。
