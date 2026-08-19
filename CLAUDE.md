# CLAUDE.md

本文件为 Claude Code（claude.ai/code）在本仓库工作时提供指引。

## 项目概述

本仓库是 AXBot 聊天机器人的对外文档站点，基于 MkDocs Material 构建。内容为面向用户的中文文档：使用说明、命令列表、数据介绍、赞助信息、开发计划等。

**注意：文档是对外公开的。** 不要写入敏感信息（内部服务地址、配置、密钥等），也不要写入尚未向用户发布的功能。

## 常用命令

- `uv sync`：安装依赖（依赖定义在 `pyproject.toml`，锁定在 `uv.lock`）
- `make dev`：本地预览（`mkdocs serve --livereload`）
- `make build`：构建站点（`mkdocs build`），**提交改动前的必做验证**
- `make build-image`：构建 Docker 镜像 `axbot-qq-docs:latest`

本仓库没有单元测试，`make build` 即为验证手段——它能发现 MkDocs 配置错误、导航条目失效、Markdown 扩展问题。涉及页面布局或导航的改动，还应 `make dev` 在浏览器中实际检查。

## 仓库结构

- `docs/`：文档源文件
  - 顶层页面：`index.md`、`sponsor.md`、`report.md`、`roadmap.md`
  - `docs/user-guide/`：用户指南（平台使用、雷币说明等）
  - `docs/command-list/`：命令列表（基础、娱乐、战争雷霆、QQ 群、订阅命令）
  - `docs/data-intro/`：数据介绍
  - 图片放在 `docs/images/`、`docs/assets/` 及各栏目自己的 `images/` 目录
- `mkdocs.yml`：站点导航、主题、Markdown 扩展配置——**新增页面要加入 nav 才会出现在菜单中**
- `overrides/`：主题覆盖（`main.html`）
- `site/`：构建产物，不要提交
- `Dockerfile` + `default.conf`：以 nginx 托管 `site/` 产物

## 内容规范

- 全部内容为中文，术语与相邻页面保持一致
- Markdown 文件名使用小写 kebab-case（如 `new-feature-guide.md`）
- `docs/` 内部使用相对链接；图片就近放在使用它的栏目目录下，共享资源除外
- 写作风格：简洁 Markdown、清晰的标题层级、短段落，必要时给出示例

## 提交规范

- 提交信息用中文，带简洁的约定式前缀，如 `docs: 更新赞助说明`
- PR 需说明改动了哪些页面；涉及布局、图片、主题的改动附截图，并在 PR 描述中确认 `make build` 通过

## CI/CD

`.github/workflows/ci.yml` 在推送到 `main` 时依次执行：`mkdocs gh-deploy` 发布到 GitHub Pages → 构建并推送 Docker 镜像到阿里云 ACR → SSH 到自建服务器 `docker compose up -d` 部署。因此本地 `make build` 必须通过。

## 安全注意

- 不要提交凭据、部署密钥、本地环境文件
- Docker 仓库登录、自建服务器部署使用 GitHub Actions secrets
- 命令列表文档以 `axbot-qq` 代码仓库（位于本地 `D:\Projects\wt-data-insight\axbot-qq`）为准，但只记录已对用户发布的功能
