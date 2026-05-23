---
title: "Hugo 建站完全指南｜三级系列总目录"
date: 2026-05-23
lastmod: 2026-05-23
draft: false
description: "Hugo 建站三级系列完整目录：初级、中级、高级，从零到上线。"
keywords: ["Hugo建站教程", "Hugo Stack主题", "Hugo中文教程"]
url: "hugo-guide"
layout: "page"
menu:
    main:
        name: Hugo建站指南
        weight: -90
        params:
            icon: book
---

> 这是「Hugo 建站完全指南」系列的总目录页。  
> 内容来自我自己从 **Notion + NotionNext** 迁移到 **Hugo + Stack + Cloudflare Pages** 的真实经历——踩过的坑、走通的路，都在这里了。  
> 按你现在的情况，找到对应起点，直接开始读。

---

## 为什么会有这个系列？

我曾经用 Notion + NotionNext + Vercel 搭博客，结果：

- Vercel 端构建三番五次报错，网站说挂就挂，直接影响运营
- 想改个样式，得先搞懂 Next.js，维护成本极高
- 内容全部锁在 Notion，依赖第三方 API，随时可能失控

最后下定决心换到 Hugo。折腾一圈之后发现：**Hugo 才是个人博客的最优解**——构建极快、部署免费、高度可控、内容完全归自己。

把踩过的坑整理成三个层级，按你的需求找对应起点。

---

## 📋 三级系列总览

| 系列 | 目标 | 适合人群 | 费用 |
|------|------|---------|------|
| 🟢 初级 | 跑通核心流程，网站搭并上线 | Hugo 零基础新手 | 完全免费 |
| 🟡 中级 | 风格自定义 + 功能扩展 | 已跑通初级流程 | 免费教程 + 可选资源包 |
| 🔴 高级 | 完全自定义模板 | 有一定基础，想深入者 | 规划中 |

---

## 🟢 初级系列｜从零到上线

**目标：** 用 Hugo+Stack主题+Cloudflare Pages，搭建免费、自动部署、全球可访问的个人博客。  
**完成后你能得到：** 一个真实在线的个人博客，`git push` 即自动更新，托管完全免费。

---

### 📄 第 1 篇｜为什么选 Hugo？从 Notion + NotionNext 踩坑说起

从我自己的真实踩坑经历出发，对比 WordPress、Hexo、Notion+NotionNext、Astro 等方案的本质区别，帮你在开始之前就做好正确的选型判断。

👉 [1-2026 年为什么还选 Hugo？从 Notion + NotionNext 踩坑说起](/2026-hugo-why/)

**适合你，如果：** 还在纠结用什么工具建站，或者已经被其他方案折腾过

---

### 📄 第 2 篇｜Hugo 本地环境搭建 + 创建第一个站点

手把手在 Windows 上安装 Hugo Extended 版本、Git、VS Code，创建第一个本地站点，搞懂目录结构，跑通 `hugo server` 本地预览。包含新手最常遇到的 5 个报错解决方案。

👉 [2-Hugo v0.158 本地安装 + 创建第一个站点（Windows 完整图文教程）](/hugo-v0158-local-install/)

**适合你，如果：** 准备正式开始，需要把本地环境配好

> ⚠️ 注意：必须安装 **Extended** 版本，普通版后续编译 SCSS 会直接报错

---

### 📄 第 3 篇｜Stack 主题安装与基础配置

安装 Hugo Stack 主题，配置侧边栏、导航菜单、头像、中文语言包，创建归档、搜索、关于等固定页面。包含 exampleSite 快速上手方案和 4 大常见报错解决。

👉 [3-Stack 主题安装与基础配置完整教程（2026 最新）](/2026-best-hugo-themes/)

**适合你，如果：** Hugo 已经装好，开始正式配置主题

---

### 📄 第 4 篇｜写文章全攻略：Front Matter、分类标签、封面图

Front Matter 完整字段说明（含中文注释模板）、分类与标签体系设计、封面图尺寸规范、Markdown 常用语法速查，以及如何用 Shortcode 嵌入 YouTube 视频。

👉 [4-Hugo 写文章全攻略：Front Matter、Markdown 语法、封面图与文章模板](/hugo-writing-guide/)

**适合你，如果：** 主题配好了，准备写第一篇文章

---

### 📄 第 7 篇｜Hugo + Cloudflare Pages 自动化部署 ⭐ 最实用

把本地 Hugo 站推送到 GitHub，通过 Cloudflare Pages 实现 `git push` 自动全球部署。

**本篇重点补充：**
- `.git` 子模块天坑（新手最高频的部署失败原因）
- `HUGO_VERSION` 为什么必须手动设置
- 自定义域名绑定（Cloudflare 域名 vs 其他平台域名两种情况）
- 5 大常见部署报错完整解决方案

👉 [7-Hugo + Cloudflare Pages 自动化部署完整教程（git push 即全球上线，2026 最新）](/hugo-cloudflare-pages-deploy/)

**适合你，如果：** 本地已经跑通，准备把网站真正上线

---

> 💡 **初级推荐阅读顺序：** 第1篇 → 第2篇 → 第3篇 → 第4篇 → 第7篇  
> 第 5、6、8 篇内容偏深，跑通基础流程后再看效果更好。

---

## 🟡 中级系列｜自定义风格与功能

**目标：** 在初级基础上，把网站改造成有自己风格的样子，并扩展实用功能。  
**完成后你能得到：** 一个有独特视觉风格、功能完整的个人网站。

---

### 📄 第 5 篇｜自定义 Shortcode 实战

创建 4 个实用 Shortcode：彩色提示框、卡片式链接、图片画廊、Bilibili 视频嵌入。包含完整 HTML 模板代码和 SCSS 样式，复制即用。同附 Hugo 内置 Shortcode 速查表。

👉 [5-Hugo 自定义 shortcode 实战：常见嵌入功能（YouTube、图片画廊、代码高亮等）](/hugo-custom-shortcode/)

---

### 📄 第 6 篇｜主题深度美化：custom.scss 覆盖 + 主题色 + 暗色模式

通过 `custom.scss` 修改主题色、字体大小、侧边栏 Widget 样式，实现图标与标题并排、暗色模式细节优化。**不需要改动主题源码，主题升级也不会丢失修改。** 附完整 `custom.scss` 起步模板。

👉 [6-Hugo 美化进阶：custom.scss 覆盖主题样式、主题色定制与暗黑模式调优](/hugo-tailwind-dark-mode/)

---

### 📄 第 8 篇｜性能优化与 SEO 进阶

配置 Sitemap 更新频率、robots.txt 自定义、Google Search Console 完整接入步骤、Open Graph 标签验证、Lighthouse 检测与优化建议，以及 Cloudflare Analytics 使用说明。

👉 [8-Hugo 性能优化与 SEO 进阶：Sitemap、robots.txt、图片优化与 Google Search Console](/hugo-performance-seo/)

---

## 🔴 高级系列｜完全自定义（规划中）

**目标：** 完全脱离现成 Hugo 主题，深度定制模板和功能。<br>
**适合：** 已熟悉中级内容，想进一步深入 Hugo 模板开发的用户。

### 规划中的内容方向

- Hugo 模板开发原理（layouts / partials / baseOf）
- 从零构建完全自定义主题
- Hugo + AI 自动化写作与部署工作流
- 个人博客的 SEO 增长与流量变现路径

> 📌 高级系列正在制作中，关注
> [YouTube @OneSmallStepMe](https://www.youtube.com/@OneSmallStepMe)
> 不错过更新

---

## 🗺️ 快速选择入口

| 我的情况 | 从这里开始 |
|---------|-----------|
| 完全没用过 Hugo，想了解值不值得学 | [第 1 篇：为什么选 Hugo](/2026-hugo-why/) |
| 直接上手，跳过选型分析 | [第 2 篇：本地环境搭建](/hugo-v0158-local-install/) |
| 本地已跑通，需要上线 | [第 7 篇：Cloudflare Pages 部署](/hugo-cloudflare-pages-deploy/) |
| 网站已上线，想让它好看一点 | [第 6 篇：主题深度美化](/hugo-tailwind-dark-mode/) |
| 想写更丰富的文章格式 | [第 5 篇：自定义 Shortcode](/hugo-custom-shortcode/) |
| 想被搜索引擎收录、提升流量 | [第 8 篇：性能优化与 SEO](/hugo-performance-seo/) |

---

## ❓ 常见问题

**Q：完全没有编程基础，能跟上吗？**  
A：初级系列完全没有编程要求，按步骤操作就行。中级系列需要会复制粘贴代码，高级系列才需要真正的开发基础。

**Q：整套搭下来要花多少钱？**  
A：初级系列完全免费——Hugo 免费、Stack 主题免费、Cloudflare Pages 免费托管。唯一花费是域名（约 ¥50-100/年），不买也可以用 Cloudflare 分配的免费子域名先用着。

**Q：我用的是 Mac，教程适用吗？**  
A：本系列基于 Windows 环境写作，命令行有少量差异，但核心流程完全一致，文章中会标注差异之处，Mac 用户跟着做没有问题。

**Q：Stack 主题更新后，教程还有效吗？**  
A：本系列基于 Stack 最新版（配合 Hugo v0.158.0+）编写，`lastmod` 会记录最近更新时间。遇到版本差异欢迎在评论区留言。

---

## 💬 遇到问题？

- 每篇文章底部评论区，直接留言
- YouTube 视频评论区（每篇文章对应一期视频，持续更新中）
- 👉 [YouTube @OneSmallStepMe](https://www.youtube.com/@OneSmallStepMe)

---

*持续更新中 · 最后更新：2026-05-23*
