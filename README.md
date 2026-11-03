# Prompt

提示词模板站点：内置「产品自动迭代智能体」全文，支持只读预览与一键复制。

## 功能

- 展示完整提示词（来源：`public/template.md`）
- 一键复制
- 说明待办扫描：`is:issue state:open archived:false assignee:@me sort:updated-desc`
- 说明已有 PR 跟进：检查 pending / unresolved 评论与 Changes Requested，有则在原分支解决
- Favicon：`public/favicon.svg` / `favicon.png`

## 地址

- https://prompt.liduchuan.com
- https://prompt-liduchuan.alex-lichangnan.workers.dev

## 架构

本项目使用 **Cloudflare Workers Static Assets** 部署：

- **`public/`** 目录下的所有文件通过 Workers Static Assets 直接提供服务
- **`public/template.md`** 是提示词模板的**唯一来源**
- **`public/index.html`** 在页面加载时通过 `fetch('/template.md')` 动态获取模板内容
- **`src/index.js`** 仅作为 Workers 入口，将所有请求转发给 `ASSETS`

## 如何更新提示词

**直接编辑 `public/template.md`，然后部署即可。** 站点页只做预览与复制，不再要求填写目标仓库 URL。

部署：

```bash
npx wrangler deploy
```

提示词核心约定：

1. 扫描分配给当前 GitHub 登录用户的 open issues（`assignee:@me`），不再维护固定目标仓库列表。
2. 若 issue 已有相关 open PR：跟进 pending / unresolved 评论，禁止另开第二 PR。
3. 提示词保持模型无关：不绑定特定模型、IDE 或 GitHub 操作身份；PR 生命周期使用当前环境可用的 GitHub 手段（含本机 `gh`）。
