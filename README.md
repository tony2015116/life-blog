# MGUO 的生活笔记 

[访问生活笔记](https://life.mguo.dpdns.org/) · [技术博客](https://blog.mguo.dpdns.org/) · [个人主页](https://mguo.dpdns.org/)

记录生活、常用流程（SOP）和家常菜谱。和技术博客用同一套 blogdown + Hugo Jane 样式，强调色换成暖色以示区分。

## 一、栏目

| 栏目 | 目录 | 网址 | 列表页排序 |
| --- | --- | --- | --- |
| 生活 | `content/life/` | `/life/年/月/短名/` | 按时间倒序 |
| SOP | `content/sop/` | `/sop/短名/` | 按标题 |
| 菜谱 | `content/recipes/` | `/recipes/短名/` | 卡片，按时间倒序 |

首页显示三个栏目的入口和最近更新。三个栏目共用「标签」。

## 二、本地预览

双击打开 `life-blog.Rproj`。首次使用时安装依赖（和技术博客相同，已装过可跳过）：

```r
install.packages("blogdown")
blogdown::install_hugo(version = "0.170.0", extended = FALSE)
```

运行 `blogdown::serve_site()` 预览，结束时运行 `blogdown::stop_server()`。

## 三、新建文章

在 RStudio 选择 **Addins → New Post**：

1. **Subdirectory** 选栏目：`life`、`sop` 或 `recipes`（默认是 `life`）。
2. **Archetype** 选同名模板：`life`、`sop` 或 `recipes`。模板会预先填好该栏目需要的字段和小节。
3. 填标题和英文短名（Slug），格式选 **Markdown**。

也可以在 Console 里新建：

```r
blogdown::new_post("番茄牛腩", kind = "recipes", subdir = "recipes", slug = "beef-tomato")
blogdown::new_post("月度家庭对账", kind = "sop", subdir = "sop", slug = "monthly-budget")
```

新文章默认 `draft: true`，确认后改为 `draft: false` 再提交。

### 菜谱字段

```yaml
servings: 2            # 几人份
prep_time: 10 分钟
cook_time: 20 分钟
total_time: 30 分钟     # 显示在菜谱卡片上
difficulty: 简单        # 简单 / 中等 / 费工
ingredients:
  - {group: 主料}       # 分组标题，可省略
  - {name: 番茄, amount: 2 个}
  - {group: 调料}
  - {name: 盐, amount: 3 克}
```

封面：把图片命名为 `cover.jpg`（或 `.png`、`.webp`）放进文章文件夹，列表卡片和正文顶部都会显示，卡片图会自动裁成 640×420。没有封面时用菜名首字作占位。

### SOP 字段

```yaml
lastmod: 2026-11-19    # 每次修改步骤时更新，显示为「最后更新」
scope: 适用场景
frequency: 每月 1 日
duration: 20 分钟
version: 2             # 改动较大时加一，并在文末「变更记录」写一行
toc: true              # 显示目录
```

正文里的 `- [ ] 事项` 会显示成可勾选的清单，刷新页面后清空。

### 生活字段

`location` 可选，填了会显示在日期后面。

## 四、发布

推送到 `main` 后，GitHub Actions 用 Hugo 0.170.0 生成并发布到 GitHub Pages。

首次上线需要：

1. 在 GitHub 新建仓库（如 `tony2015116/life-blog`），推送本项目。
2. 仓库 **Settings → Pages → Source** 选 **GitHub Actions**，自定义域名填 `life.mguo.dpdns.org`。
3. DNS 加一条记录：

   | 类型 | 名称 | 目标 | Cloudflare 代理 |
   | --- | --- | --- | --- |
   | CNAME | `life` | `tony2015116.github.io` | 仅 DNS |

4. 证书签发后启用 **Enforce HTTPS**。

## 五、常用维护位置

| 要修改的内容 | 文件或目录 |
| --- | --- |
| 栏目名称和简介 | `content/<栏目>/_index.md` |
| 「关于」页 | `content/about.md` |
| 网址、标题、导航、侧栏链接 | `config.yaml` |
| 新文章模板 | `archetypes/` |
| 顶栏、侧栏、页脚 | `layouts/_default/baseof.html` |
| 首页 | `layouts/index.html` |
| 文章页、菜谱信息框、SOP 信息框 | `layouts/_default/single.html`、`layouts/partials/life/` |
| 列表页 | `layouts/_default/section.html`、`layouts/sop/`、`layouts/recipes/` |
| 基础样式（与技术博客一致） | `static/css/custom.css` |
| 本站专用样式和配色 | `static/css/life.css` |
