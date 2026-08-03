# Hank 的博客

个人博客，基于 **Hexo**，托管在 **GitHub Pages**，域名 **https://hank.l.cd**。

---

## 📖 给 AI 的操作手册（新对话请先读这里）

> 当用户（Hank）说"发文章/写博客/上传文章"时，按下面的流程执行。

### 基本信息

| 项目 | 值 |
|---|---|
| 仓库 | `rthdfd/rthdfd.github.io`（默认分支 `main`） |
| 博客框架 | Hexo 8（Node.js 22，依赖通过 npm 安装） |
| 主题 | landscape（npm 依赖形式，非 themes/ 目录） |
| 自定义域名 | `hank.l.cd`（CNAME → `rthdfd.github.io`，标记文件在 `source/CNAME`） |
| 部署方式 | GitHub Actions 自动构建（`.github/workflows/deploy.yml`），**push 到 main 即自动发布**，无需手动构建 |

### 发文章的标准流程

1. 在 `source/_posts/` 下创建 Markdown 文件（用 GitHub MCP 的 `create_or_update_file` 或 `push_files`）
2. 文章格式（YAML frontmatter + Markdown 正文）：

```markdown
---
title: 文章标题
date: 2026-08-03 12:00:00
tags:
  - 标签一
categories:
  - 分类一   # 必须是下方「分类管理」中 category_list 已注册的分类
---

正文内容，用 Markdown 写……
```

3. 提交到 `main` 分支（commit message 用中文，如 `📝 发布新文章：xxx`）
4. 等 GitHub Actions 跑完（约 2~3 分钟），文章会出现在 `https://hank.l.cd/年/月/日/标题/`

### 分类管理（预留接口）

- 所有分类**集中注册**在 `_config.yml` 的 `category_list` 中，目前只有：`实用网站`
- **新增分类**只需两步：① 在 `category_list` 中新增一项（name/desc）→ ② 新文章的 front-matter `categories` 引用该 name
- 分类聚合入口：导航菜单「Categories」→ `/categories/`，侧边栏也会自动展示所有分类
- 博客的默认分类是 `实用网站`（`scaffolds/post.md` 模板已预设）
- **删除文章** = 删除 `source/_posts/` 下对应文件。注意：当前 GitHub MCP 工具无法直接删除远程文件，需要用户在 GitHub 网页上手动删除，或把文件改为 `published: false` 停用

### 关键文件说明

| 文件 | 作用 |
|---|---|
| `_config.yml` | 站点配置：标题、作者、URL、**`category_list` 分类注册表**（加分类改这里） |
| `source/_posts/*.md` | **所有文章**（在这里新增/修改） |
| `source/CNAME` | 自定义域名标记，**不要删** |
| `.github/workflows/deploy.yml` | 自动部署脚本，**不要删** |
| `scaffolds/` | 文章模板（hexo new 用） |

### 注意事项

- `node_modules/` 和 `public/` 已被 .gitignore 排除，**不要提交**
- 图片：先上传到 `source/images/` 目录，文章中用 `![](/images/xxx.png)` 引用
- 文件名建议用英文/拼音（如 `my-first-post.md`），中文文件名会导致 URL 被编码
- 修改 `_config.yml` 的 `url` 后要确认是 `https://hank.l.cd`，不要改回 github.io
- 部署是 Actions 自动完成的：**AI 只负责改 source/ 下的文件并 push，不要手动生成 public/**

---

## 📝 给 Hank 自己看的

### 怎么发文章（不用碰命令行）

1. 打开仓库：https://github.com/rthdfd/rthdfd.github.io
2. 进入 `source/_posts/`
3. 点 **Add file → Create new file**，按上面的模板写
4. **Commit changes** → 等 2~3 分钟 → 去 https://hank.l.cd 看

### 或者：直接把内容发给 AI

把想写的内容直接发给 AI（任意新对话），AI 会读这份 README 并帮你完成上传和发布。

---

## ⚙️ 技术栈

- **框架**：Hexo 8
- **托管**：GitHub Pages（免费）
- **部署**：GitHub Actions（免费）
- **域名**：hank.l.cd（DNSHE 免费域名）
- **主题**：landscape（可替换为 Butterfly / Next 等）
