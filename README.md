# steller09

独立于 steller08 的全新项目，面向 Cloudflare Workers / OpenNext / Next.js 的高尔夫 AI 教练平台。

## 核心主链路
- 注册 / 登录
- 创建学员
- 上传视频
- 创建 session
- 轻分析
- 深分析
- 报告生成
- D1 写入
- 历史可见
- Pro 查看学员和分析记录

## 本地命令
```bash
git clone https://github.com/dytsui/steller09.git
cd steller09
pwd
# 应看到类似: .../steller09

npm install
npm run build:web
npm run typecheck:web
npm run open-next:build
```

如果你误进了其他目录（例如 `my-new-project`），`npm run` 只会看到默认 `test` 脚本，
这时请先 `cd` 回本仓库根目录（含 `apps/`, `cloudflare/`, `docs/`）。

## 部署
见 `docs/deploy.md`。
