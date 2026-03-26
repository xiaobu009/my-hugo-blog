---
title: "2-Hugo v0.158 本地安装 + 创建第一个站点"
date: 2026-03-26T09:37:23+08:00
draft: false
description: ""
url: "hugo-v0158-local-install"
categories:
    - Hugo建站
tags:
    - Hugo
    - 静态站点
    - 建站教程
---


在了解了 Hugo 的强大之后，现在我们正式开始动手实践。本文将手把手带你在各个主流操作系统（Windows/Mac/Linux）上安装最新鲜出炉的 **Hugo v0.158**，并创建你的第一个 Hugo 基本站点结构。

## 1. 安装 Hugo v0.158

请务必安装 `Hugo extended` （扩展版），以便能够正常处理 SASS/SCSS 等样式文件（这在诸如 Stack 等大多数主流主题中是强依赖的）。

### Windows 平台安装

如果你使用的是 Windows 10/11，推荐使用包管理工具 Scoop 或 Winget 安装。

**使用 Scoop (推荐)：**
```bash
scoop install hugo-extended
```

**使用 Winget：**
```bash
winget install Hugo.Hugo.Extended
```

### macOS 平台安装

Mac 用户请直接使用 Homebrew：
```bash
brew install hugo
```

### Linux 平台安装

对于 Ubuntu / Debian 系，建议直接从 [Hugo GitHub Releases](https://github.com/gohugoio/hugo/releases) 下载官方的 `.deb` 安装包，然后执行：
```bash
sudo dpkg -i hugo_extended_0.158.0_linux-amd64.deb
```

### 验证安装

在终端/命令行中输入以下命令以确认版本：
```bash
hugo version
```

> **注意事项：**
> 只要输出中包含类似 `v0.158.0` 和 `+extended` 字样，即代表安装成功。

## 2. 创建你的首个 Hugo 站点

打开命令行，切换到你准备存放博客源码的目录（比如 `C:\web\`），运行：

```bash
hugo new site my-hugo-blog
```

此时会生成一个名为 `my-hugo-blog` 的新文件夹。其核心目录结构如下：
1. `archetypes/`：文章模板定义
2. `content/`：这里存放你所有的 Markdown 文章（很重要！）
3. `layouts/`：自定义 HTML 结构
4. `static/`：存放静态资源如图片、CSS 和 JavaScript 
5. `themes/`：存放所有的第三方主题
6. `hugo.toml`：全站的核心配置文件

<!--![新建Hugo站点结构](images/hugo-new-site-struct.png) -->

> **注意事项：**
> 刚建好的站点是没有任何样式的，直接运行 `hugo server` 会看到一片空白。这是因为我们还没有为站点安装任何“主题（Theme）”。

---

## 系列文章导航

我们已经成功迈出了本地环境搭建的重要一步，接下来的重点就是“装修”我们的网站了。

👉 **本系列下一篇预告：** [2026 最推荐 3 个 Hugo 主题实操：PaperMod / Stack / Hugo Blox](./2026-best-hugo-themes.md)

**查看全系列教程：** 返回 [smallstep.top 系列导航提示](/categories/hugo建站/) 查看所有文章！
