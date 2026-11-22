# Clash 订阅管理（公开展示）

[查看展示页](https://hyjack-00.github.io/clash_subscription_public/) · [查看分组 Prompt](prompt.md) · [后台管理与操作说明](https://github.com/hyjack-00/clash_subscription_private)（仅仓库授权用户）

```mermaid
flowchart TB
  A[私有：多个上游订阅与规则] --> B[私有：GitHub Actions 合并、重构并加密]
  B --> C[公开：GitHub 仓库与 Pages 展示]
  C --> D[Cloudflare Worker：解密并提供下游订阅]
  D --> E[Clash 客户端]
  classDef private fill:#e9efff,stroke:#526ac2,color:#14244a;
  classDef public fill:#e4f6eb,stroke:#39835b,color:#163d29;
  classDef cloud fill:#fff0df,stroke:#d47a18,color:#52300d;
  classDef client fill:#f1f1f1,stroke:#777,color:#222;
  class A,B private;
  class C public;
  class D cloud;
  class E client;
```

## 名称

- **上游订阅**：供应商提供的一个或多个私密链接。
- **下游订阅密钥**：用于解密最终订阅，后台推荐 Secret 名为 `DOWNSTREAM_SUB_KEY`（兼容旧名 `SUBSCRIPTION_KEY`）。
- **下游订阅地址**：`https://clash-subscription-private.hyjack285.workers.dev/s/<下游订阅密钥>`，填写到 Clash；完整链接相当于密码。

## 管理入口

- **更换上游订阅**：[私有仓库 Secrets](https://github.com/hyjack-00/clash_subscription_private/settings/secrets/actions) 中更新 `UPSTREAM_SUB_URLS`（每行一个链接）；不要把订阅地址粘贴在公开网页或仓库。
- **修改规则**：[编辑后台 Prompt](https://github.com/hyjack-00/clash_subscription_private/blob/main/config/agent_prompt.md)，提交后运行一次更新。公开 Prompt 在更新成功时同步；目前 Agent 只识别美国节点。
- **立即刷新**：打开[更新任务](https://github.com/hyjack-00/clash_subscription_private/actions/workflows/update-subscription.yml)，点 Run workflow → 选择 `main` → Run workflow。无需手动执行时每 6 小时自动运行。
- **搭建订阅入口**：打开 [Cloudflare](https://dash.cloudflare.com/)，按[后台 Worker 指南](https://github.com/hyjack-00/clash_subscription_private/blob/main/worker/README.md)操作。

本仓库只存加密产物和公开信息，不要提交原始订阅、节点密码或密钥。状态统计在首次更新成功后生成。添加上游后才能生成真实订阅；上游规则尚未合并。
