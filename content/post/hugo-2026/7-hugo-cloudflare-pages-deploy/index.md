---
title: "7-Hugo + Cloudflare Pages 自动化部署完整教程（git push 即全球上线，2026 最新）"
date: 2026-03-26T09:37:23+08:00
draft: false
description: "手把手教你将 Hugo 博客托管至 Cloudflare Pages。借助 GitHub 实现自动化部署，享受免费且极致的边缘节点全球加速访问体验。"
url: "hugo-cloudflare-pages-deploy"
categories:
    - Hugo建站
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

## 7. 告别手动上传，拥抱现代化部署

前六篇教程中，我们的网站仅运行在本地。今天是激动人心的一步：让全世界都能访问我们的博客！

在 2026 年，静态资源托管领域非常成熟。经过对比（Vercel, Netlify, GitHub Pages），本次教程我们重点推荐 **Cloudflare Pages**。它提供极其慷慨的免费额度、不计量带宽，以及 Cloudflare 引以为傲的全球 CDN 加速节点。

我们将实现这样的工作流：**本地写文章 -> git push 提交到 GitHub -> Cloudflare 自动拉取构建 -> 全球瞬间上线更新**。

---

## 7.1 将本地项目推送到 GitHub

1. 在 GitHub 上新建一个**私有或公开的 Repository**(仓库)，比如命名为 `my-hugo-blog`。
2. 回到你的本地博客根目录（如 `C:\web\smallstep.stack`），初始化并推送：

```bash
git init
git add .
git commit -m "Initial commit for Hugo blog"
git branch -M main
git remote add origin https://github.com/你的用户名/my-hugo-blog.git
git push -u origin main
```

> [!WARNING]
> **注意事项（关于主题的 Git Clone）：**
> 由于我们的 Stack 主题是通过 `git clone` 下载的，它的目录带有 `.git` 文件夹。在将整个博客提交到你的仓库前，建议**删掉 `themes/hugo-theme-stack/.git` 隐藏文件夹**，将其变为普通文件夹。否则 Git 会把它当做 Submodule，导致 Cloudflare Pages 拉取不到主题文件而构建失败！这是绝大多数新手部署失败的第一大天坑！

<!-- ![Git 推送成功截图](git-push.png) -->

---

## 7.2 链接 Cloudflare Pages

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)，在左侧导航栏选择 **Workers & Pages**。
2. 点击 **Create (创建)**，选择 **Pages -> Connect to Git**。
3. 授权连接你的 GitHub 账号，并选择刚才创建的 `my-hugo-blog` 仓库。
4. 点击 **Begin setup**。

### 关键构建设置（2026版）

在设置页面，请必须核对以下参数（非常重要）：
- **Project name**: 填入你喜欢的名称，这将作为默认分配的三级域名。
- **Production branch**: `main`
- **Framework preset**: 选择 `Hugo`（会自动填入相关命令）。
- **Build command**: 默认是 `hugo`。如果遇到版本问题，请确保使用 `hugo --minify` 进行压缩构建。
- **Build output directory**: 保持默认的 `public`。

**环境变量（Environment Variables）设置（核心提示）：**
展开环境变量设置，添加一条记录：
- **Variable name**: `HUGO_VERSION`
- **Value**: `0.158.0` （指定云端构建环境与我们本地一致，否则 Cloudflare 可能使用旧版 Hugo 导致我们用到的 css.Build 新特性报错）。

<!-- ![Cloudflare Pages 构建配置截图](cf-pages-setup.png) -->

---

## 7.3 一键起飞！

点击 **Save and Deploy**。Cloudflare 会分配一台容器拉取你的源码，安装 Hugo v0.158.0，并在几秒钟内完成构建。
随后，你会得到一个类似 `your-blog.pages.dev` 的访问链接。

从今天起，你只需要每次写完文章，执行 `git push`，剩下的全交给自动化！下一步，我们还可以直接在 Cloudflare 后台绑定自己的个性化独立域名（如 `smallstep.top`）。

---

## 系列导航

- **上一篇回顾：** [6-Hugo 美化进阶：Tailwind CSS + 暗黑模式 + 自定义样式](/hugo-tailwind-dark-mode/)
- **下一篇预告：** [8-Hugo 性能优化与 SEO 进阶：Hugo Pipes、sitemap、robots.txt 等](/hugo-performance-seo/)

> **返回 SmallStep 系列教程目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/)
