# my-docs

个人技术文档站：用 Markdown + MkDocs Material 写内容，由 GitHub Actions 自动构建并部署到 GitHub Pages。

- **网站地址**：<https://Jing-Zhan.github.io/my-docs/>
- **个人实验报告**：[report/README.md](report/README.md)
- **作者**：占竞（Jing-Zhan），学号 202430224058

## 项目结构

```text
my-docs/
├── .github/workflows/deploy.yml   # 推送 docs/ 后自动构建并部署到 Pages
├── docs/                          # 网站内容（四页 Markdown）
│   ├── index.md                   # 首页
│   ├── about.md                   # 关于本项目
│   ├── guide.md                   # 使用指南
│   └── api.md                     # API 参考
├── report/                        # 个人实验报告与关键步骤截图
├── mkdocs.yml                     # 站点名、主题、导航配置
├── requirements.txt               # 构建依赖
└── README.md
```

## 本地预览

```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1      # Windows PowerShell；macOS/Linux 用 source .venv/bin/activate
python -m pip install -r requirements.txt
python -m mkdocs serve            # 打开 http://127.0.0.1:8000/
```

严格构建（有警告即失败，与 CI 行为一致）：

```bash
python -m mkdocs build --strict
```

## 自动部署

推送包含 `docs/**`、`mkdocs.yml`、`requirements.txt` 或工作流文件的提交到 `main` 分支即可自动触发：

```bash
git add docs
git commit -m "更新文档"
git push origin main
```

在仓库的 **Actions** 页面可以看到 `Deploy MkDocs to GitHub Pages` 的运行记录，`build` 与 `deploy` 均成功后，网站内容即更新。也可以在 Actions 页面手动运行（`workflow_dispatch`）。

## 说明

- `site/` 是构建产物，已加入 `.gitignore`，不需要手写或提交。
- 只修改 `report/` 或 `README.md` 不会触发部署，这是 `paths` 过滤的预期行为。
- 完整的操作过程、截图与原理说明见 [个人实验报告](report/README.md)。
