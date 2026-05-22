---
title: "6-Hugo 美化进阶：custom.scss 覆盖主题样式、主题色定制与暗黑模式调优"
date: 2026-03-28T19:38:23+08:00
lastmod: 2026-05-17
draft: false
description: "深入讲解 Hugo Stack 主题的样式覆盖机制，通过 custom.scss 实现主题色自定义、字体调整、暗黑模式细节优化，以及侧边栏 Widget 标题图标位置修改。无需改动主题源码，升级主题也不会丢失修改。"
keywords: ["Hugo Stack 自定义样式", "custom.scss Hugo", "Hugo 主题色修改", "Hugo 暗黑模式", "Hugo Stack 美化", "Hugo SCSS 覆盖"]
url: "hugo-tailwind-dark-mode"
categories:
    - station
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

博客上线了，主题也装好了，但总觉得哪里不够"自己的"？

这一篇教你用 `custom.scss` 对 Stack 主题进行深度定制——修改主题色、调整字体大小、优化暗黑模式细节，以及我们在实际使用中真正改过的那些样式调整。**全程不需要动主题源码**，即使以后升级主题，你的修改也不会丢失。

---

## 6.1 Stack 主题的样式覆盖机制

Stack 主题预留了一个专门给用户自定义样式的入口：

```
assets/scss/custom.scss
```

这个文件如果不存在，新建一个就行。Hugo 在编译时会把这个文件的内容**追加到主题样式的最后**，利用 CSS 的级联优先级自动覆盖主题默认样式。

**这个机制的好处：**

- **不修改主题源码**：主题文件夹里的任何内容都不需要动
- **主题升级安全**：`git pull` 更新主题不会覆盖你的自定义样式
- **即时生效**：`hugo server` 运行时，修改 `custom.scss` 保存后浏览器自动刷新

**文件位置：**

```
你的博客根目录/
└── assets/
    └── scss/
        └── custom.scss    ← 所有自定义样式都写在这里
```

> ⚠️ **注意是站点根目录的 `assets/`，不是 `themes/` 里面。** 写到主题目录里的修改会在主题升级时被覆盖。

---

## 6.2 Stack 主题的 CSS 变量体系

Stack 主题的颜色、间距等核心样式都通过 CSS 变量定义，我们直接覆盖变量就能批量修改外观，不需要逐条找选择器。

常用的 CSS 变量（在 `custom.scss` 里覆盖即可）：

```scss
:root {
    // 主题强调色（链接、按钮、高亮等）
    --accent-color: #3b82f6;
    --accent-color-darker: #2563eb;

    // 字体大小基准（整站字体缩放）
    --article-font-size: 1.6rem;

    // 卡片背景色
    --card-background: #fff;
    --card-separator-color: rgba(218, 218, 218, 0.5);

    // 侧边栏背景
    --sidebar-background-color: #f6f6f6;
}

// 暗色模式变量覆盖
[data-scheme="dark"] {
    --accent-color: #60a5fa;
    --card-background: #1e2030;
    --sidebar-background-color: #161b2e;
}
```

---

## 6.3 修改主题色

Stack 默认的强调色是蓝色系。如果你想改成其他颜色，只需要覆盖两个变量：

**改成绿色系（清新技术感）：**

```scss
:root {
    --accent-color: #10b981;
    --accent-color-darker: #059669;
}

[data-scheme="dark"] {
    --accent-color: #34d399;
    --accent-color-darker: #10b981;
}
```

**改成橙色系（温暖个人风格）：**

```scss
:root {
    --accent-color: #f97316;
    --accent-color-darker: #ea580c;
}

[data-scheme="dark"] {
    --accent-color: #fb923c;
    --accent-color-darker: #f97316;
}
```

> 💡 **颜色选择建议：** 推荐去 [Tailwind CSS 调色板](https://tailwindcss.com/docs/customizing-colors) 挑颜色，所有颜色都有深浅梯度，亮色模式用 `500`，暗色模式用 `400` 通常效果不错。

---

## 6.4 调整字体大小

Stack 主题的字体大小基于 `rem` 单位，根元素（`html`）的字体大小决定整站基准。

**让全站文字略微放大（适合内容较多的博客）：**

```scss
// 桌面端
@media (min-width: 1024px) {
    :root {
        font-size: 62.5%;    // 默认是 62.5%，即 1rem = 10px
    }
}

// 文章正文字体大小
.article-content {
    font-size: 1.7rem;       // 默认 1.6rem，调大一点
    line-height: 1.85;       // 行距也适当放宽
}
```

**调整标题大小：**

```scss
.article-content {
    h2 { font-size: 2.2rem; }
    h3 { font-size: 1.9rem; }
    h4 { font-size: 1.7rem; }
}
```

---

## 6.5 右侧 Widget 标题样式调整

Stack 主题默认的 Widget 标题（如"精选文章"、"分类"、"标签云"）图标在上、文字在下，上下排列。如果你想让图标和文字左右并排显示，需要覆盖 Widget 模板并加样式。

**第一步：在 `custom.scss` 里加入 Widget 标题 flex 布局**

```scss
// Widget 标题区域：图标和文字左右并排
.widget {
    .widget-header {
        display: flex;
        align-items: center;
        gap: 8px;
        margin-bottom: 14px;

        .widget-icon {
            margin: 0;
            flex-shrink: 0;
            display: flex;
            align-items: center;

            svg {
                width: 24px;
                height: 24px;
            }
        }

        h2.widget-title {
            margin: 0;
            font-size: 2rem;      // 标题字体大一号
            line-height: 1;
        }
    }
}
```

**第二步：覆盖 Widget 模板文件**

Stack 主题的每个 Widget 都有对应的 HTML 模板，需要把图标和标题包进一个 `.widget-header` 容器里。

以归档 Widget 为例，复制主题文件到站点目录：

```bash
# 在站点根目录执行
copy themes\hugo-theme-stack\layouts\_partials\widget\archives.html layouts\partials\widget\archives.html
```

打开复制过来的文件，把原来的结构：

```html
<!-- 原来：图标和标题分开，上下排列 -->
<div class="widget-icon">
    <svg>...</svg>
</div>
<h2 class="widget-title section-title">归档</h2>
```

改成：

```html
<!-- 改后：用 .widget-header 包裹，flex 左右并排 -->
<div class="widget-header">
    <div class="widget-icon">
        <svg>...</svg>
    </div>
    <h2 class="widget-title section-title">归档</h2>
</div>
```

对 `categories.html`（或其依赖的 `taxonomy.html`）和 `tag-cloud.html` 做同样的修改，自定义的 `featured.html` 也保持相同结构。

> 💡 **为什么要复制到站点目录？** Hugo 的模板查找优先级是：站点 `layouts/` > 主题 `layouts/`。把文件复制到站点目录后修改，主题升级时不会覆盖你的改动。

---

## 6.6 暗黑模式细节优化

Stack 主题的暗色模式原生支持已经很完整，但有几个地方可以做得更精致：

**代码块在暗色模式下的背景色：**

```scss
[data-scheme="dark"] {
    .highlight {
        background-color: #1a1b2e !important;
    }

    // 行内代码背景
    code:not(.noHighlight):not([class*="language-"]) {
        background-color: rgba(255, 255, 255, 0.08);
        color: #e2e8f0;
    }
}
```

**图片在暗色模式下轻微降低亮度（保护眼睛）：**

```scss
[data-scheme="dark"] {
    .article-content img {
        filter: brightness(0.9);
        transition: filter 0.3s;

        &:hover {
            filter: brightness(1);  // 鼠标悬停时恢复原始亮度
        }
    }
}
```

**卡片阴影在暗色模式下的调整：**

```scss
[data-scheme="dark"] {
    .card-wrapper {
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.4);

        &:hover {
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.5);
        }
    }
}
```

---

## 6.7 其他实用样式调整

**文章列表摘要字体大小：**

```scss
.article-subtitle {
    font-size: 1.4rem;
    opacity: 0.75;
}
```

**文章内表格样式增强（Stack 主题默认表格样式较基础）：**

```scss
.article-content {
    table {
        width: 100%;
        border-collapse: collapse;
        margin: 1.5rem 0;
        font-size: 1.4rem;

        th {
            background-color: var(--accent-color);
            color: #fff;
            padding: 10px 14px;
            text-align: left;
            font-weight: 600;
        }

        td {
            padding: 9px 14px;
            border-bottom: 1px solid var(--card-separator-color);
        }

        tr:nth-child(even) td {
            background-color: rgba(0, 0, 0, 0.02);
        }

        tr:hover td {
            background-color: rgba(59, 130, 246, 0.05);
        }
    }
}

[data-scheme="dark"] {
    .article-content {
        tr:nth-child(even) td {
            background-color: rgba(255, 255, 255, 0.03);
        }
    }
}
```

**首页文章列表卡片的封面图高度（默认偏矮）：**

```scss
// 让首页卡片封面图更高，视觉更舒展
.article-list--tile article .article-image img {
    height: 220px;
    object-fit: cover;
}
```

**侧边栏文章标题字体放大：**

```scss
.widget-body a {
    font-size: 1.5rem;
    line-height: 1.6;
}
```

---

## 6.8 修改后的 custom.scss 完整模板

把上面所有调整整合在一起，下面是一份适合中文技术博客的 `custom.scss` 完整起步模板，按需取用：

```scss
// ===== 1. 主题色 =====
:root {
    --accent-color: #3b82f6;
    --accent-color-darker: #2563eb;
}

[data-scheme="dark"] {
    --accent-color: #60a5fa;
}

// ===== 2. 字体 =====
.article-content {
    font-size: 1.7rem;
    line-height: 1.85;

    h2 { font-size: 2.2rem; }
    h3 { font-size: 1.9rem; }
}

// ===== 3. Widget 标题图标并排 =====
.widget {
    .widget-header {
        display: flex;
        align-items: center;
        gap: 8px;
        margin-bottom: 14px;

        .widget-icon {
            margin: 0;
            flex-shrink: 0;
        }

        h2.widget-title {
            margin: 0;
            font-size: 2rem;
        }
    }
}

// ===== 4. 暗色模式细节 =====
[data-scheme="dark"] {
    .article-content img {
        filter: brightness(0.9);
        &:hover { filter: brightness(1); }
    }

    code:not(.noHighlight):not([class*="language-"]) {
        background-color: rgba(255, 255, 255, 0.08);
    }
}

// ===== 5. 表格增强 =====
.article-content {
    table {
        width: 100%;
        border-collapse: collapse;
        margin: 1.5rem 0;
        font-size: 1.4rem;

        th {
            background-color: var(--accent-color);
            color: #fff;
            padding: 10px 14px;
            font-weight: 600;
        }

        td {
            padding: 9px 14px;
            border-bottom: 1px solid var(--card-separator-color);
        }

        tr:nth-child(even) td {
            background-color: rgba(0,0,0,0.02);
        }
    }
}
```

---

## 本篇小结

通过 `custom.scss`，你已经掌握了 Stack 主题样式定制的核心方法：

- ✅ 理解了 Stack 主题的样式覆盖机制，改动安全不影响主题升级
- ✅ 学会了通过 CSS 变量批量修改主题色
- ✅ 调整了字体大小和行距，让中文阅读更舒适
- ✅ 优化了暗黑模式下的图片、代码块和卡片阴影细节
- ✅ 增强了表格样式，让文章内的对比表格更清晰

下一篇是部署篇——把本地博客推送到 GitHub，连接 Cloudflare Pages 实现自动化全球部署。

---

## 系列导航

- **上一篇：** [5-Hugo 自定义 Shortcode 实战：提示框、卡片链接、图片画廊](/hugo-custom-shortcode/)
- **下一篇：** [7-Hugo + Cloudflare Pages 自动化部署完整教程](/hugo-cloudflare-pages-deploy/)
- **返回系列目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/categories/station/)
