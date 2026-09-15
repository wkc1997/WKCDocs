# 审计文库（MaoDocs）复刻版

复刻自 [https://docs.maoyanqing.com/](https://docs.maoyanqing.com/) 的注册会计师常用法律法规库，用于**内部分发学习**。

## 技术栈

- [VitePress](https://vitepress.dev/)（与原站同框架）
- 本地全文搜索
- 亮/暗双主题

## 内容结构

- **会计**：会计法、企业会计准则、小企业会计准则、企业会计制度、政府会计准则制度、非营利组织会计制度、基金类会计制度、农村集体会计制度等
- **审计**：注册会计师法、职业道德守则、独立性准则、执业准则、应用指南、问题解答、地方注协提示等
- **证券**：证券法、交易所业务规则、监管规则指引、上市公司监管指引、会计监管风险提示等
- **内控**：企业/小企业/行政事业单位内部控制规范
- **评估**：资产评估法、资产评估准则、专家指引、操作指引等

已收录的法规全文包括：**《会计法》（2024）、《证券法》（2019）、《资产评估法》（2016）、《中国注册会计师审计准则第1101号（2022）》**。其余条目以准确摘要 + 官方来源链接呈现。

## 本地运行

```bash
npm install
npm run docs:dev      # 开发预览 http://localhost:5173
npm run docs:build    # 生产构建，输出到 docs/.vitepress/dist
npm run docs:preview  # 构建后预览
```

## 部署到 GitHub Pages（免费公开）

### 方案一：手动推送

1. 在 GitHub 新建公开仓库 `WKCDocs`；
2. 在本地项目根目录执行：
   ```bash
   git init
   git add .
   git commit -m "init: 审计文库复刻版"
   git remote add origin https://github.com/wkc1997/WKCDocs.git
   git push -u origin main
   ```
3. 由于 `base` 已设为 `/WKCDocs/`，需在 GitHub 仓库 Settings → Pages → Source 选择 `GitHub Actions`（推荐）或 `gh-pages` 分支。

### 方案二：GitHub Actions（自动部署，推荐）

在项目根目录创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy VitePress site to Pages
on:
  push:
    branches: [main]
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: pages
  cancel-in-progress: false
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: npm
      - run: npm ci
      - run: npm run docs:build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: docs/.vitepress/dist
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

> ⚠️ 部署后访问地址为：`https://wkc1997.github.io/WKCDocs/`
> 若你要用 `https://wkc1997.github.io/`（个人主页根路径）直接访问，则需将 `config.mjs` 中 `base: '/WKCDocs/'` 改为 `base: '/'`，并把仓库名改为 `wkc1997.github.io`。

## 致谢与版权

- 内容整理结构与框架参考 [审计文库（MaoDocs）](https://docs.maoyanqing.com/)，作者 **毛燕庆**（© 2026，苏ICP备18066969号）；
- 本复刻版仅用于**内部分发学习**，法规正文为国家公开法律法规（不适用著作权保护）；
- 若涉及商业或对外公开使用，请遵循原站版权声明并与原作者沟通。