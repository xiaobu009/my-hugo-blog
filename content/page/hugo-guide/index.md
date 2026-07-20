---
title: "Hugo 建站完全指南｜三级系列总目录"
date: 2026-05-23
lastmod: 2026-05-30
draft: false
description: "Hugo 建站三级系列完整目录：初级从零到上线，中级个性化定制，高级自定义模板开发。"
keywords: ["Hugo建站教程", "Hugo Stack主题", "Hugo中文教程", "Hugo入门", "个人博客搭建"]
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
| 🟢 初级 | 跑通核心流程，网站搭起来并上线 | Hugo 零基础新手 | 完全免费 |
| 🟡 中级 | 风格自定义 + 功能扩展 | 已跑通初级流程 | 完全免费 |
| 🔴 高级 | 从零开发自定义主题模板 | 有一定基础，想深入者 | 规划中 |

---

## 🟢 初级系列｜从零到上线

**目标：** 用 Hugo + Stack 主题 + Cloudflare Pages，搭建免费、自动部署、全球可访问的个人博客。  
**完成后你能得到：** 一个真实在线的个人博客，`git push` 即自动更新，托管完全免费。

---

### 📄 第 1 篇｜Hugo 本地环境搭建

安装 Hugo Extended 版本和 Git，创建第一个本地站点，安装 Stack 4.0 主题，完成基础配置，在浏览器里看到自己的博客首页。包含新手最常遇到的报错解决方案。

👉 [零基础搭建个人博客——Hugo + Stack 4.0 本地环境完整指南](/hugo-local-setup/)

**适合你，如果：** 准备从零开始，需要把本地环境配好

> ⚠️ 必须安装 **Extended** 版本，普通版编译 SCSS 会直接报错

---

### 📄 第 2 篇｜部署上线 + 绑定域名 ⭐ 最实用

把本地 Hugo 站推送到 GitHub，通过 Cloudflare Pages 实现 `git push` 自动全球部署，绑定自定义域名。

👉 [免费把博客发布到全球——GitHub + Cloudflare Pages 部署完整指南](/hugo-cloudflare-deploy/)

**本篇重点：**

- `.git` 子模块天坑（新手最高频的部署失败原因）
- `HUGO_VERSION` 为什么必须手动设置
- 自定义域名绑定（Cloudflare 域名 vs 其他平台两种情况）
- 日常更新博客的标准流程


**适合你，如果：** 本地已经跑通，准备把网站真正上线

---

> 💡 **初级推荐阅读顺序：** 第1篇 → 第2篇，按顺序来，不要跳

---

## 🟡 中级系列｜自定义风格与功能

**目标：** 在初级基础上，把网站改造成有自己风格的样子，并扩展实用功能。  
**完成后你能得到：** 一个有独特视觉风格、功能完整的个人网站。

---

### 📄 中级01｜个性化配置

配置头像、个人介绍、社交媒体链接、侧边栏组件、导航菜单。纯配置操作，不涉及代码修改。

👉 [让博客更像你的：Stack 主题个性化配置指南](/hugo-stack-config/)

---

### 📄 中级02｜写作体验优化

Front Matter 字段说明、文章模板设置、内容目录改名，让写文章这件事变得顺手。

👉 [写作体验优化：Front Matter、文章模板与内容目录改名](/hugo-frontmatter-cover-image/)

---

### 📄 中级03｜主题视觉定制

通过 `custom.scss` 修改主题色、字体、暗色模式细节。**不改主题源码，主题升级也不会丢失修改。**

👉 [主题视觉定制：custom.scss 覆盖样式、主题色、暗色模式](/hugo-stack-custom-style)

---

### 📄 中级04｜流量与变现基础

Google Analytics 接入、Google Search Console 配置、AdSense 申请条件与植入方式。网站上线后让它被统计、被找到、能赚钱。

👉 [流量与变现基础——Google Analytics、Search Console、AdSense 起步指南](https://smallstep.one/hugo-analytics-adsense-basics/)

---

### 📄 中级05｜SEO 进阶（即将发布）

Sitemap 配置、robots.txt 自定义、Open Graph 标签验证、结构化数据。让搜索引擎更好地收录你的内容。
👉 [SEO 进阶——让 Google 和社交平台真正「看懂」你的博客](/hugo-seo-advanced/)

---

### 📄 中级06｜评论系统接入（即将发布）

评论系统接入（Giscus / Disqus）。

---

### 📄 中级07｜功能扩展（即将发布）

多语言基础配置、搜索功能优化。

---

## 🔴 高级系列｜完全自定义（规划中）

**目标：** 使用 Hugo 框架，从零开始创建完全自定义的主题模板并部署。  
**适合：** 已熟悉中级内容，想彻底掌控网站每一个细节的用户。

> 📌 高级系列正在规划中，关注 [YouTube @OneSmallStepMe](https://www.youtube.com/@OneSmallStepMe) 不错过更新

---

## 🗺️ 快速选择入口

| 我的情况 | 从这里开始 |
|---------|-----------|
| 完全没用过 Hugo，想从头搭建 | 初级01：[本地环境搭建](/hugo-local-setup/) |
| 本地已跑通，需要上线 | 初级02：[Cloudflare Pages 部署](/hugo-cloudflare-deploy/) |
| 网站已上线，想配置头像和社交链接 | 中级01：[Stack 主题个性化配置指南](/hugo-stack-config/) |
| Front Matter，模板设置、目录改名 | 中级02：[写作体验优化](/hugo-frontmatter-cover-image/) |
| 想让网站好看一点 | 中级03： [主题视觉定制](/hugo-stack-custom-style/) |
| 想接入 Google 统计和 AdSense | 中级04： [流量与变现基础](https://smallstep.one/hugo-analytics-adsense-basics/) |
| 想被搜索引擎更好收录 | 中级05：[SEO 进阶](/hugo-seo-advanced/) |
| 评论系统接入 | 中级06：（即将发布） |
| 多语言配置 + 站内搜索优化 | 中级07：（即将发布） |
---

## ❓ 常见问题

**Q：完全没有编程基础，能跟上吗？**  
A：初级和中级系列完全没有编程要求，按步骤操作就行。高级系列才需要真正的开发基础。

**Q：整套搭下来要花多少钱？**  
A：初级和中级完全免费——Hugo 免费、Stack 主题免费、Cloudflare Pages 免费托管。唯一可能的花费是域名（约 ¥50-100/年），不买也可以用 Cloudflare 分配的免费子域名先用着。

**Q：我用的是 Mac，教程适用吗？**  
A：本系列基于 Windows 环境写作，命令行有少量差异，但核心流程完全一致，文章中会标注差异之处，Mac 用户跟着做没有问题。

**Q：Stack 主题更新后，教程还有效吗？**  
A：本系列基于 Stack 4.0（配合 Hugo v0.158.0）编写，`lastmod` 字段会记录最近更新时间。遇到版本差异欢迎在评论区留言。

---

## 💬 遇到问题？

- 发邮件：contact@smallstep.one
- YouTube 视频评论区（每篇文章对应一期视频）
- 👉 [YouTube @OneSmallStepMe](https://www.youtube.com/@OneSmallStepMe)

---

*持续更新中 · 最后更新：2026-05-30*
