+++
date = '2025-11-20T15:58:05+08:00'
draft = false
title = '使用 Cloudflare Pages 搭建个人博客的完整指南'
description = '详细介绍如何使用Cloudflare Pages和Hugo搭建免费、快速、稳定的个人博客，包括从安装Hugo到部署上线的全过程。'
+++

# 使用 Cloudflare Pages 搭建个人博客的完整指南

Cloudflare Pages 是 Cloudflare 提供的免费静态网站托管服务，支持无缝集成 GitHub/GitLab，自动构建部署，提供全球 CDN 加速、免费 SSL 证书、无限带宽和请求（免费额度内）。它非常适合搭建个人博客，尤其是结合静态站点生成器（如 Hugo、Hexo、Astro 或 Jekyll），速度快、成本为零，且国内访问体验优于 GitHub Pages。

下面以最流行的 **Hugo**（推荐：生成速度极快、Go 语言编写）为例，详细说明步骤。如果你更喜欢 Hexo（Node.js  기반），步骤类似，只需调整构建命令。

## 前提条件
- 一个 GitHub 账号（用于托管源码）。
- 一个 Cloudflare 账号（免费注册即可：[https://dash.cloudflare.com](https://dash.cloudflare.com)）。
- （可选）自定义域名（可在阿里云、腾讯云或 Namesilo 购买，建议几元/年的 .com 或 .top）。

## 步骤 1: 安装 Hugo 并创建本地博客
1. 下载 Hugo（推荐 extended 版，支持 Sass 等高级功能）：
   - Windows/Mac/Linux 官网下载：[https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)（选择最新版本，如 hugo_extended_0.134.0 或更高）。
   - 安装后在终端运行 `hugo version` 验证。

2. 创建新站点：
   ```
   hugo new site my-blog  # 创建名为 my-blog 的文件夹
   cd my-blog
   ```

3. 添加主题（推荐美观主题，如 Stack 或 PaperMod）：
   - 使用 git 子模块添加（推荐，便于更新）：
     ```
     git init  # 如果还没初始化 git
     git submodule add [https://github.com/CaiJimmy/hugo-theme-stack/](https://github.com/CaiJimmy/hugo-theme-stack/)  themes/hugo-theme-stack
     ```
   - 或者直接 clone 主题到 themes 文件夹。

4. 配置 config.toml（或 config.yaml）：
   - 复制主题的 exampleSite/config.toml 到根目录。
   - 修改基本信息：
     ```toml
     baseURL = "[https://yourname.pages.dev/](https://yourname.pages.dev/)"  # 先用 Cloudflare 的子域名
     title = "我的个人博客"
     theme = "hugo-theme-stack"  # 根据你的主题修改
     languageCode = "zh-cn"
     ```

5. 创建第一篇文章：
   ```
   hugo new posts/my-first-post.md
   ```
   - 编辑 content/posts/my-first-post.md（Markdown 格式）。

6. 本地预览：
   ```
   hugo server -D
   ```
   - 浏览器访问 http://localhost:1313 查看效果。

## 步骤 2: 推送到 GitHub
1. 创建 GitHub 仓库（名称随意，如 my-blog），可以私有。
2. 将本地项目推送到 GitHub：
   ```
   git add .
   git commit -m "Initial commit"
   git remote add origin [https://github.com/你的用户名/my-blog.git](https://github.com/你的用户名/my-blog.git)
   git push -u origin main  # 或 master
   ```
   - 包含 themes 子模块：使用 `git submodule update --init --recursive`。

## 步骤 3: 在 Cloudflare Pages 部署
1. 登录 Cloudflare Dashboard → Workers & Pages → Pages → Create a project。
2. 选择 "Connect to Git" → 授权 GitHub → 选择你的仓库和分支（main）。
3. 配置构建设置：
   - Framework preset: Hugo（如果检测不到，选择 None）。
   - Build command: `hugo --gc --minify`（推荐添加参数优化）。
   - Build output directory: `public`（Hugo 默认输出目录）。
   - **重要：添加环境变量**（避免版本问题）：
     - Key: `HUGO_VERSION`，Value: `0.134.0`（或你的 Hugo 版本，查看 [https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases) 最新 extended 版）。
4. 点击 "Save and Deploy"。
   - 首次部署需几分钟，成功后得到子域名：如 my-blog.pages.dev。
   - 以后 push 到 GitHub，Cloudflare 会自动重新构建部署（1-2 分钟）。

## 步骤 4: 绑定自定义域名（可选，但推荐）
1. 在域名提供商处，将域名 NS 记录指向 Cloudflare（注册时会引导，全权托管到 Cloudflare 更好，提供免费 CDN）。
2. 在 Cloudflare Pages 项目 → Custom domains → Set up a domain → 输入你的域名（如 blog.example.com）。
3. Cloudflare 会自动添加 CNAME 记录并颁发免费 SSL 证书（Full 或 Full strict 模式）。
4. 更新 Hugo 的 config.toml 中的 baseURL 为你的自定义域名，重新 push 部署。
   - 访问 [https://你的域名](https://你的域名) 即可！

## 常见替代方案：用 Hexo 搭建（如果你更熟悉 Node.js）
1. 安装 Hexo：`npm install -g hexo-cli`。
2. 创建站点：`hexo init my-blog`。
3. 添加主题（如 Next）：下载到 themes 文件夹，修改 _config.yml。
4. Cloudflare Pages 配置：
   - Build command: `hexo generate` 或 `npm install && hexo generate`。
   - Output directory: `public`。
   - 无需额外环境变量。

## 额外优化建议
- **评论系统**：用 Cusdis、Giscus（基于 GitHub Discussions）或 Utterances（免费、无后端）。
- **统计**：Cloudflare 自带 Analytics，或集成 Umami/Plausible。
- **搜索**：主题内置，或添加 Algolia/Pagefind。
- **防盗链/加速**：在 Cloudflare Dashboard → ScrapeShield 开启 Hotlink Protection。
- **图片托管**：用 Cloudflare R2（免费额度高）或外部图床。
- **免费额度**：每月 500 次构建、无限请求/带宽，个人博客完全够用。

整个过程完全免费，部署后全球访问飞快（Cloudflare 有大量国内节点）。如果遇到问题（如构建失败），查看 Pages 的部署日志，通常是 Hugo 版本不匹配或主题配置错误。

推荐参考教程：
- Hugo 官网：[https://gohugo.io](https://gohugo.io)
- Cloudflare Pages 文档：[https://developers.cloudflare.com/pages/](https://developers.cloudflare.com/pages/)

这样，你的个人博客就搭建好了！有问题欢迎在评论区讨论～
