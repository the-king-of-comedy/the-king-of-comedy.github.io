+++
date = '2026-08-04T10:21:56+08:00'
draft = true
title = 'Hugo搭建博客项目'
+++

## 前置准备与完整步骤

1. **安装 Hugo 和 Git:** Debian 环境配置.
在 Debian 终端中更新软件源，并安装 Git 与 Hugo。

```bash
# 1. 更新系统并安装基础依赖
sudo apt update
sudo apt install -y git curl wget

# 2. 安装 Hugo
sudo apt install hugo

# 3. 验证安装
hugo version

```


2. **新建 Hugo 站点与配置 Git 仓库:**
在本地创建 Hugo 项目目录，并初始化 Git 仓库：

```bash
# 创建名为 my-blog 的站点
hugo new site my-blog
cd my-blog

# 初始化 Git 仓库并设置主分支名为 main
git init
git branch -M main

```


3. **添加博客主题:**
以常用的轻量主题 **PaperMod** 为例，通过 Git Submodule（子模块）方式引入：

```bash
# 添加主题子模块
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

# 在配置文件 (hugo.toml 或 config.toml) 中指定主题
echo 'theme = "PaperMod"' >> hugo.toml

```


4. **本地预览与创建首篇文章:**
生成第一篇博文并在本地启动服务验证：

```bash
# 新建一篇文章
hugo new posts/first-post.md

# 编辑文章内容（使用 nano 或 vim）
nano content/posts/first-post.md
# 提示：需将 Front Matter 中的 draft: true 改为 draft: false 才能正式展示

# 启动本地预览服务器（包含草稿和未发布内容）
hugo server -D

```

启动后，在浏览器访问 `http://localhost:1313` 即可实时预览站点。


5. **在 GitHub 创建仓库:**
1. 登录 GitHub，创建一个名为 `<你的用户名>.github.io` 的**公开（Public）仓库**（例如 `username.github.io`）。
2. 如果使用自定义域名或私有仓库，也可以使用任意仓库名，但推荐个人主页直接使用 `username.github.io`。


6. **配置 GitHub Actions 自动构建:**
在项目根目录下创建 GitHub Actions 工作流配置文件：

```bash
mkdir -p .github/workflows
nano .github/workflows/deploy.yml

```

将以下 YAML 配置写入 `.github/workflows/deploy.yml` 文件中：

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"] # 如果你的主分支叫 master，请改为 master

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive # 必须包含此项以拉取主题
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```


7. **推送代码与开启 GitHub Pages:** 关键发布步骤.
将代码推送到 GitHub，并在仓库设置中开启 Pages 功能：

```bash
# 添加远程仓库地址（替换为你的真实 GitHub 仓库 URL）
git remote add origin git@github.com:username/username.github.io.git

# 添加文件并提交
git add .
git commit -m "feat: initial hugo blog setup"
git push -u origin main

```

**在 GitHub 上配置 Pages 来源：**

1. 进入 GitHub 仓库页面，点击 **Settings** -> **Pages**。
2. 在 **Build and deployment** 下的 **Source** 中，选择 **GitHub Actions**。
3. 切换到 **Actions** 标签页，即可看到自动化部署任务已成功触发。部署完成后，访问 `https://<用户名>.github.io` 即可看到博客。


---

## 常用日常维护命令

* **新建文章**：`hugo new posts/文章标题.md`
* **本地测试**：`hugo server -D`（在本地检查排版）
* **发布文章**：修改文章 Front Matter 中的 `draft: false`，然后执行：
```bash
git add .
git commit -m "content: add new post"
git push

```