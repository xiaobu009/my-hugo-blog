---
title: "6-Hugo 美化进阶：Tailwind CSS + 暗黑模式 + 自定义样式（Stack 主题示例）"
date: 2026-03-28T19:38:23+08:00
draft: false
description: "基于 Hugo Stack 主题，讲解如何覆盖原生样式，引入 Tailwind CSS 思想，完善暗黑模式体验，打造个性化博客。"
url: "hugo-tailwind-dark-mode"
categories:
    - Hugo建站
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

## 6. 让你的博客与众不同

Stack 是一款设计非常精美的 Hugo 主题，但在 [smallstep.top](/) 上，我们总希望能加入更多属于自己的个性化元素。在这一篇，我们将探讨如何安全地覆盖 Stack 的默认样式，以及如何利用最新的 CSS 功能增强暗黑模式。

在前面的教程中，为了方便对主题进行深度自定义，我们选择了 `git clone` 方式将 Stack 主题直接安装在 `themes/hugo-theme-stack` 目录下。这正是此刻能够大显身手的基础！

### 6.1 主题管理方式对比：Git Clone vs Hugo Module

在开始修改样式之前，简要回顾一下我们为什么选择 Git Clone 安装主题：

- **Git Clone（我们目前使用的方式）**：
  将主题完整克隆至 `themes/`，文件全部在本地可见。
  - *优点*：非常适合想要深度魔改主题结构、修改内部代码和资源结构的用户，所见即所得。
  - *缺点*：后续上游主题更新修复时，由于我们本地修改过代码，可能会产生 Git 冲突，升级稍麻烦。

- **Hugo Module（可选方式）**：
  Hugo 官方近年来主推的 Go Modules 依赖管理方案。
  - *优点*：保持项目干净，不直接拉取所有文件到本地；版本控制简单，一句命令即可更新到最新版主题。
  - *缺点*：修改核心代码需要对 Hugo 的文件覆盖机制（Lookup Order）非常熟悉，心智负担较重。

对于追求个性的我们，当前采用的深度克隆方式非常完美。下面进入正题！

---

## 6.2 自定义 CSS 的正确姿势

Stack 主题提供了一个极佳的自定义入口。无需直接修改主题源码，而是通过在项目根目录创建自定义 CSS。

1. 在项目根目录创建（或编辑） `assets/scss/custom.scss`。
2. 任何写在这里的 SCSS 代码，Hugo 都会在编译时自动覆盖主题的默认配置（得益于 Hugo 2026 主推的 css.Build 极速构建特性）。

### 实战：修改主题主色调

假设我们要将默认蓝/深灰色调整为充满科技感的“赛博朋克紫”，打开 `custom.scss`：

```scss
:root {
    --accent-color: #8b5cf6; /* 紫色 */
    --sys-color-primary: var(--accent-color);
}
```

重新运行 `hugo server`，整个站点的链接、按钮等高亮颜色将瞬间发生变化！

<!-- ![修改主题颜色对比图](color-change.png) -->

---

## 6.3 暗黑模式 (Dark Mode) 深度定制

现代博客暗黑模式是刚需。Stack 主题已经原生支持。但我们可以对细节更进一步：

通过在全局 CSS 中添加媒体查询或特定的类名，微调暗黑模式下的组件阴影和背景纯度：

```scss
[data-scheme="dark"] {
    --card-background: #111111;
    --text-color: #e5e5e5;
    
    .article-details {
        box-shadow: 0 4px 20px rgba(139, 92, 246, 0.15); /* 加点紫色光晕 */
    }
}
```

> [!WARNING]
> **注意事项：** 当你测试暗黑模式时，记得同时清理浏览器缓存，或者开启无痕窗口，确认系统级跟随 (`prefers-color-scheme`) 与手动切换按钮均正常工作。

---

## 6.4 结合 Tailwind CSS 的思路 (进阶)

得益于 Hugo v0.158 的原生能力，处理类似 Tailwind 这样的功能类库变得更加直观。如果你需要在你的 Markdown 中使用类似 Tailwind 的类名，可以在 `layouts/_default/baseof.html` 中引入轻量级的样式系统或借助 Hugo Pipes 处理。

由于篇幅限制，此处我们更推荐优先使用纯 CSS/SCSS + 自定义 Shortcode 解决 95% 的博客样式需求，保持极佳的编译速度。

---

## 系列导航

- **上一篇回顾：** [5-Hugo 自定义 shortcode 实战：常见嵌入功能](/hugo-custom-shortcode/)
- **下一篇预告：** [7-Hugo + Cloudflare Pages 自动化部署完整教程](/hugo-cloudflare-pages-deploy/)
- **返回 SmallStep 系列教程目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/categories/hugo建站/)
