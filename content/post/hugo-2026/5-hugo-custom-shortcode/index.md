---
title: "5-Hugo 自定义 shortcode 实战：常见嵌入功能（YouTube、图片画廊、代码高亮等）"
date: 2026-03-28T18:36:52+08:00
draft: false
description: "详解如何在 Hugo v0.158 中自定义 shortcode，实现灵活的嵌入功能如 YouTube 视频、图片画廊和高级代码高亮，提升博客交互体验。"
url: "hugo-custom-shortcode"
categories:
    - Hugo建站
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

## 5. 初识 Shortcode：Hugo 的魔法组件

在前面四篇教程中，我们已经成功搭建了搭载 Stack 主题的 Hugo 博客。然而，原生 Markdown 在实现一些高级页面元素时显得有些捉襟见肘，比如嵌入视频、创建图片画廊、或者是定义特殊样式的提示框。

在 2026 年的 Hugo v0.158.0 中，**Shortcodes（短代码）** 提供了一种极其优雅的解决方案。它允许你在 Markdown 文件中调用预定义好的 HTML 模板代码，非常适合深度定制你的 smallstep.top 站点。

### 5.1 Hugo 内置 Shortcode

```code
Hugo 默认已经提供了一些常用的 Shortcode，例如：
{{</* youtube w7Ft2ymGmfc */>}}：快速嵌入 YouTube 视频。
{{</* gist spf13 7896402 */>}}：嵌入 GitHub 上的 Gist 代码片段。 
{{</* figure src="cover.png" title="示例图片" */>}}：带原图和描述的响应式图片。

```

> [!TIP]
> **注意事项：** 使用 `<` 和 `>` 组合（如 `{{</* mycustom "hello" */>}}`）表示将内容作为纯 HTML 渲染；而如果是 `%`（如 `{{%/* mycustom "hello" */%}}`），则 Hugo 会将内部内容继续视为 Markdown 解析。这是很多人刚接触 shortcode 容易踩坑的地方！

---

## 5.2 自定义 Shortcode 实战

如果内置功能无法满足需求，我们可以自己写！所有自定义的短代码都保存在项目根目录的 `layouts/shortcodes/` 中。

### 实战一：创建专属的提示框 (Notice)

我们来实现一个类似本站使用的精美警告/提示框。

1. **新建文件** `layouts/shortcodes/notice.html`
2. **写入以下代码**：

```html
<div class="custom-notice notice-{{ .Get "type" }}">
    <strong>{{ .Get "title" }}：</strong>
    {{ .Inner }}
</div>
```

3. **在 Markdown 中调用**：

```markdown
{{</* notice type="warning" title="重要提醒" */>}}
这里是警告框内部的内容，支持换行！
{{</* /notice */>}}
```

<!-- ![自定义提示框效果预览](notice-preview.png) -->

### 实战二：代码高亮强化版

Hugo v0.158 内置的 Chroma 高亮已经极为强大，但如果你需要在代码块上方加上文件名和复制按钮，可以通过 shortcode 结合 JavaScript 实现。Stack 主题目前已经自带了优雅的代码高亮方案，一般情况下直接用 Markdown 的 ` ```bash ` 即可。

---

## 5.3 进阶：实现简单的图片画廊

通过传递多个图片路径到 shortcode，利用 CSS Grid 实现一个美观的照片墙。这对于展示摄影作品或多步截图非常有用。

1. **创建** `layouts/shortcodes/gallery.html`：
```html
<div class="grid grid-cols-2 md:grid-cols-3 gap-4">
  {{ range split (.Get "images") "," }}
  <img src="{{ . }}" class="rounded-lg shadow-md" alt="Gallery Image">
  {{ end }}
</div>
```

2. **页面中调用：**
```markdown
{{</* gallery images="img1.png,img2.png,img3.png" */>}}
```

---

## 系列导航

- **上一篇回顾：** [4-Hugo 写文章全攻略：front matter、分类/标签、多语言配置详解](/hugo-writing-guide)
- **下一篇预告：** [6-Hugo 美化进阶：Tailwind CSS + 暗黑模式 + 自定义样式（Stack 主题示例）](/hugo-tailwind-dark-mode/)
- **查看全系列教程：** 返回 [Hugo建站](/categories/hugo建站/) 查看所有文章！
