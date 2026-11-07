# 关于本项目

## 项目目标

实现从 Markdown 文档到公开网站的自动化发布。

具体一点说，这个项目要达到三个效果：

1. 内容只用 Markdown 写，不碰 HTML/CSS；
2. 本地能预览，和线上效果一致；
3. 推送代码后自动构建、自动部署、自动上线，不需要人工干预。

## 技术组成

| 工具 | 作用 |
| --- | --- |
| Markdown | 编写文档内容 |
| MkDocs Material | 将文档构建成静态网站 |
| GitHub Actions | 自动执行安装、构建和部署 |
| GitHub Pages | 托管并提供网站访问地址 |

## 各部分的职责边界

理解这套组合的关键，是分清“谁负责什么”。我用一张表来对比：

| 对象 | 存放位置 | 是否需要提交 | 说明 |
| --- | --- | --- | --- |
| 源码 | `docs/`、`mkdocs.yml` | 需要 | 唯一需要人工维护的内容 |
| 构建产物 | `site/` | 不需要 | 每次构建重新生成，已加入 `.gitignore` |
| 工作流定义 | `.github/workflows/deploy.yml` | 需要 | 描述“怎么构建、怎么部署” |
| 托管网站 | GitHub Pages | 不需要 | 由部署动作写入，对外提供访问地址 |

所以整个系统的数据流是一条直线：

```text
docs/*.md + mkdocs.yml
        │  mkdocs build --strict
        ▼
      site/（静态 HTML/CSS/JS）
        │  upload-pages-artifact
        ▼
   Actions artifact
        │  deploy-pages
        ▼
   GitHub Pages ──► 公开网址
```

## 工作流做了什么

`deploy.yml` 分成两个任务：

- **build**：检出代码 → 配置 Python 3.12 → 安装 `requirements.txt` 里的依赖 → `mkdocs build --strict` 构建 → 把 `site/` 打包成 Pages 产物。
- **deploy**：依赖 `build` 成功（`needs: build`），拿到 `github-pages` 环境的写权限，把产物发布到 Pages。

只有 `docs/`、`mkdocs.yml`、`requirements.txt` 和工作流文件变化时才触发，改报告或 README 不会白白消耗一次构建。

## 我的实践体会

第一次配这套流程时，我原以为“部署”是把文件传到服务器某个目录。实际做下来才意识到：这里根本没有传统意义上的服务器，Pages 只是一个替我托管静态文件的仓库，而 Actions 就是一个按事件触发、跑一次就结束的临时机器。

由此得到两个具体结论：

1. **构建失败的代价很低，但必须让失败容易发现。** 因为每次是全新的虚拟环境，依赖版本、脚本语法、前端资源缺失都会在这里暴露。开 `--strict` 就是为了不让“警告”变成线上的断链。
2. **权限要最小化。** 整个工作流默认只有 `contents: read`，只有 `deploy` 任务单独拿到 `pages: write` 和 `id-token: write`。这比“给仓库开全局读写”安全得多，也更容易解释给审阅的人听。

返回 [首页](index.md)，或继续看 [使用指南](guide.md)、[API 参考](api.md)。
