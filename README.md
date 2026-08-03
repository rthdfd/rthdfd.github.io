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
| 主题 | landscape（`themes/landscape/` 含自定义模板，CI 构建时自动补全完整主题，见「主题机制」） |
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
- 分类聚合入口：导航菜单「分类」→ `/categories/`（由 `themes/landscape/layout/categories.ejs` 自动列出所有分类）
- 博客的默认分类是 `实用网站`（`scaffolds/post.md` 模板已预设）
- **删除文章** = 删除 `source/_posts/` 下对应文件。注意：当前 GitHub MCP 工具无法直接删除远程文件，需要用户在 GitHub 网页上手动删除，或把文件改为 `published: false` 停用

### 主题机制（重要，防白屏）

- 主题源在 npm 包 `hexo-theme-landscape`（`^1.0.0`），仓库 `themes/landscape/` 只放**自定义模板**（目前只有 `layout/categories.ejs` 分类聚合页模板、`layout/home.ejs` 首页模板）
- **Hexo 只要发现 `themes/landscape/` 目录存在，就以它为主题源**（不再用 node_modules 的完整主题）。若该目录不完整（缺 layout/ 模板、source/ 样式），构建出的页面是**无模板空壳 → 网站白屏**
- 因此 `.github/workflows/deploy.yml` 中有一步 `Complete theme directory`（`cp -rn node_modules/hexo-theme-landscape/. themes/landscape/`），**构建前自动补全主题目录；`-n` 不覆盖已有自定义模板，此步骤不要删除**
- 本地验证构建时同理：若输出出现 `WARN No layout`，先执行 `cp -r node_modules/hexo-theme-landscape/. themes/landscape/` 再 `hexo generate`
- 新增自定义模板时，只往 `themes/landscape/layout/` 加文件，不要动其他主题文件
- **首页模板**：`themes/landscape/layout/home.ejs`（tab 结构：文章/实用网站 + hero 横幅），由 `_config.yml` 的 `index_generator.layout: home` 指定。**该文件顶部 front-matter `layout: false` 不能删**（自包含页面，删了首页会套上旧主题外壳）。面板逻辑：「文章」tab = 非「实用网站」分类的文章；「实用网站」tab = 该分类文章，文章 front-matter 加 `link:` 变外链（新窗口）、加 `description:` 自定义描述
- **侧边栏已全局关闭**：`_config.yml` 的 `theme_config.sidebar: false`（分类/归档/最新文章等 widget 不显示），不要改回 true
- **文章前后篇导航**：`themes/landscape/layout/_partial/post/nav.ejs` 为自定义版——只关联「文章」分类（非实用网站）的文章，边界显示「已经是最后一篇了/已经是第一篇了」；实用网站文章（外链卡片）不参与导航
- **页面顶部导航已精简**：文章页/归档页 header 删除了三条杠（汉堡菜单）、RSS 图标、搜索按钮；导航菜单为中文（首页/归档/分类），配置在 `theme_config.menu`（键名即显示文字）。**当前无搜索功能**，如需要可加本地搜索插件

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
