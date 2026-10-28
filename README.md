# prompt-liduchuan

提示词模板站点：内置「产品自动迭代智能体」全文，通过填写目标仓库 URL 生成可复制提示词。

## 功能

- 多仓库 URL 输入（可添加/删除，支持粘贴多行）
- 实时预览完整提示词
- 一键复制
- GitHub URL 基础校验
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

**直接编辑 `public/template.md`，然后部署即可。** 无需修改任何其他文件。

部署后，新的模板内容会立即生效：

```bash
npx wrangler deploy
```

模板文件使用 `{{REPOS}}` 占位符，运行时会被用户填写的仓库列表替换。
