# 站点构建与部署

在线地址：<https://deng-shihao.github.io/grammar-rl/>

站点使用 MkDocs + Material 主题生成。仓库不维护两套文档副本，站点正文**逐字取自仓库中的原始 Markdown**：
`scripts/build_docs.py` 按 `mkdocs.yml` 的 `nav` 配置将原始文档复制到 `build/docs/`，
并将 `README.md` 转换为站点首页 `build/docs/index.md`（末尾自动追加可点击的站内目录），阅读样式由 `assets/reading.css` 提供。

## 本地预览

```bash
uv venv .venv && uv pip install -r requirements.txt   # 首次安装依赖
python3 scripts/build_docs.py                         # 组装 build/docs
mkdocs serve                                          # 启动本地预览：http://127.0.0.1:8000
```

修改原始 Markdown 或 `mkdocs.yml` 的 `nav` 后，需重新执行 `python3 scripts/build_docs.py` 将改动同步至 `build/docs/`（`mkdocs serve` 仅监听 `build/docs/`，不会自动重跑组装脚本）。

## 构建产物

```bash
python3 scripts/build_docs.py && mkdocs build --strict
# 静态站点输出目录：build/site/
```

## 自动发布

推送到 `main` 分支后，由 `.github/workflows/deploy.yml` 自动构建并部署至 GitHub Pages
（仓库 Pages 构建源设置为 **GitHub Actions**）；也可在 GitHub Actions 页面手动触发 `workflow_dispatch`。

## 收录规则

- **纳入发布**：`大纲/`、`真题/`、`知识点-词汇语法/`、`练习/`、`research/` 目录下的全部 Markdown 文档；`README.md` 作为站点首页。
- **不纳入发布**：`work/`（本地语料工作区，可由 EPUB 重建）、`*.epub`（电子书原文件）、`AGENTS.md`、`SITE.md`、`scripts/`、`assets/`（仅作为站点静态资源复制）、`build/`（构建输出目录）。
- **完整性校验**：仓库内除排除名单外的所有 `.md` 文件均须登记在 `mkdocs.yml` 的 `nav` 中；若有未登记文件或路径缺失，`scripts/build_docs.py` 会直接报错退出。
