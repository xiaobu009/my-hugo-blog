---
title: "5-Hugo 自定义 shortcode 实战：常见嵌入功能（YouTube、图片画廊、代码高亮等）"
date: 2026-03-28T18:36:52+08:00
lastmod: 2026-05-17
draft: false
description: "手把手教你创建 4 个实用的 Hugo 自定义 Shortcode：彩色提示框、卡片式链接、图片画廊、Bilibili 视频嵌入。包含完整 HTML 模板代码和 SCSS 样式，复制即用。"
keywords: ["Hugo shortcode 自定义", "Hugo 提示框", "Hugo 卡片链接", "Hugo 图片画廊", "Hugo Bilibili 嵌入", "layouts/shortcodes", "Hugo 短代码教程"]
url: "hugo-custom-shortcode"
categories:
    - station
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

Markdown 语法简洁，但有时候你会遇到它做不到的事：想在文章里加一个醒目的警告框、想把链接展示成好看的卡片、想嵌入一个 Bilibili 视频……

这就是 **Shortcode（短代码）** 存在的意义。它让你用简短的标签调用预定义的 HTML 模板，既保持 Markdown 文件的干净，又能实现复杂的视觉效果。

这篇教程带你动手创建 4 个最实用的自定义 Shortcode，代码全部可以直接复制使用。

---

## 5.1 Shortcode 的基础原理

**创建规则很简单：**

在站点根目录的 `layouts/shortcodes/` 下新建一个 HTML 文件，文件名就是 Shortcode 的名称。

```
layouts/
└── shortcodes/
    ├── notice.html      ← 用 {{</* notice */>}} 调用
    ├── linkcard.html    ← 用 {{</* linkcard */>}} 调用
    ├── gallery.html     ← 用 {{</* gallery */>}} 调用
    └── bilibili.html    ← 用 {{</* bilibili */>}} 调用
```

**两种调用方式：**

```
{{</* shortcode-name 参数 */>}}           ← 单标签，适合无内容的短代码
{{</* shortcode-name 参数 */>}}内容{{</* /shortcode-name >}}  ← 双标签，适合有内容包裹的
```

**两种参数传入方式：**

```
{{</* notice warning */>}}              ← 位置参数，按顺序传入
{{</* linkcard url="https://..." title="标题" */>}}   ← 命名参数，按名称传入
```

**在 Shortcode 模板里获取参数：**

```html
{{ .Get 0 }}           <!-- 获取第一个位置参数 -->
{{ .Get "url" }}       <!-- 获取名为 url 的命名参数 -->
{{ .Inner }}           <!-- 获取双标签之间的内容 -->
```

> ⚠️ **注意：** Hugo v0.146.0 之后，Shortcode 文件推荐放在 `layouts/_shortcodes/` 目录（注意下划线），但 `layouts/shortcodes/`（无下划线）依然兼容。本教程使用无下划线版本，两者均可正常工作。

---

## 5.2 实战一：彩色提示框（notice）

这是技术博客里最常用的 Shortcode 之一，用来突出显示重要信息、警告、小技巧等。

**最终效果：**

```
{{</* notice info */>}}
这是一个信息提示框，适合补充说明和小技巧。
{{</* /notice */>}}

{{</* notice warning */>}}
这是警告框，适合标注容易踩坑的地方。
{{</* /notice */>}}

{{</* notice success */>}}
这是成功提示框，适合标注操作完成确认。
{{</* /notice */>}}

{{</* notice error */>}}
这是错误提示框，适合标注常见错误。
{{</* /notice */>}}
```

**第一步：创建 `layouts/shortcodes/notice.html`**

```html
{{- $type := .Get 0 | default "info" -}}
{{- $icons := dict
    "info"    "ℹ️"
    "warning" "⚠️"
    "success" "✅"
    "error"   "❌"
-}}
{{- $icon := index $icons $type | default "ℹ️" -}}

<div class="notice notice-{{ $type }}">
  <div class="notice-icon">{{ $icon }}</div>
  <div class="notice-content">
    {{ .Inner | markdownify }}
  </div>
</div>
```

**第二步：在 `assets/scss/custom.scss` 里添加样式**

```scss
// 提示框通用样式
.notice {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px 16px;
  border-radius: 6px;
  margin: 1.5rem 0;
  font-size: 1.4rem;
  line-height: 1.7;

  .notice-icon {
    font-size: 1.6rem;
    flex-shrink: 0;
    margin-top: 1px;
  }

  .notice-content p:last-child {
    margin-bottom: 0;
  }
}

// 各类型颜色
.notice-info {
  background-color: rgba(59, 130, 246, 0.1);
  border-left: 4px solid #3b82f6;
}

.notice-warning {
  background-color: rgba(245, 158, 11, 0.1);
  border-left: 4px solid #f59e0b;
}

.notice-success {
  background-color: rgba(16, 185, 129, 0.1);
  border-left: 4px solid #10b981;
}

.notice-error {
  background-color: rgba(239, 68, 68, 0.1);
  border-left: 4px solid #ef4444;
}

// 暗色模式兼容
[data-scheme="dark"] {
  .notice-info    { background-color: rgba(59, 130, 246, 0.15); }
  .notice-warning { background-color: rgba(245, 158, 11, 0.15); }
  .notice-success { background-color: rgba(16, 185, 129, 0.15); }
  .notice-error   { background-color: rgba(239, 68, 68, 0.15); }
}
```

**使用示例：**

```
{{</* notice warning */>}}
**推送前必须**删除主题目录里的 `.git` 文件夹，否则 Cloudflare 构建会失败。
{{</* /notice */>}}
```

---

## 5.3 实战二：卡片式链接（linkcard）

普通的 Markdown 链接 `[文字](URL)` 只显示一行蓝色文字。卡片式链接可以展示标题、描述和图标，点击率更高，适合推荐工具、文章、项目等。

**最终调用方式：**

```
{{</* linkcard
  url="https://github.com/CaiJimmy/hugo-theme-stack"
  title="hugo-theme-stack"
  description="Card-style Hugo theme with a sidebar"
  image="https://avatars.githubusercontent.com/u/8169?s=48"
*/>}}
```

**创建 `layouts/shortcodes/linkcard.html`**

```html
{{- $url   := .Get "url" -}}
{{- $title := .Get "title" | default $url -}}
{{- $desc  := .Get "description" | default "" -}}
{{- $image := .Get "image" | default "" -}}

<a href="{{ $url }}" target="_blank" rel="noopener noreferrer" class="link-card">
  {{ if $image }}
  <div class="link-card-image">
    <img src="{{ $image }}" alt="{{ $title }}" loading="lazy">
  </div>
  {{ end }}
  <div class="link-card-body">
    <div class="link-card-title">{{ $title }}</div>
    {{ if $desc }}
    <div class="link-card-desc">{{ $desc }}</div>
    {{ end }}
    <div class="link-card-url">{{ $url }}</div>
  </div>
  <div class="link-card-arrow">→</div>
</a>
```

**在 `assets/scss/custom.scss` 里添加样式**

```scss
.link-card {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 14px 16px;
  border: 1px solid var(--card-separator-color);
  border-radius: 8px;
  margin: 1.5rem 0;
  text-decoration: none !important;
  color: inherit !important;
  transition: box-shadow 0.2s, border-color 0.2s;
  background: var(--card-background);

  &:hover {
    box-shadow: 0 2px 12px rgba(0,0,0,0.08);
    border-color: var(--accent-color);
  }

  .link-card-image {
    flex-shrink: 0;
    img {
      width: 48px;
      height: 48px;
      border-radius: 6px;
      object-fit: cover;
    }
  }

  .link-card-body {
    flex: 1;
    min-width: 0;
  }

  .link-card-title {
    font-size: 1.5rem;
    font-weight: 600;
    margin-bottom: 2px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .link-card-desc {
    font-size: 1.3rem;
    opacity: 0.65;
    margin-bottom: 4px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .link-card-url {
    font-size: 1.2rem;
    opacity: 0.4;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .link-card-arrow {
    font-size: 1.8rem;
    opacity: 0.3;
    flex-shrink: 0;
  }
}
```

---

## 5.4 实战三：图片画廊（gallery）

当一篇文章需要展示多张截图时，逐行插入图片会让页面很长。图片画廊 Shortcode 可以把多张图片排列成网格，点击单张图片放大查看。

**调用方式：**

```
{{</* gallery */>}}
  img/screenshot-1.png
  img/screenshot-2.png
  img/screenshot-3.png
{{</* /gallery */>}}
```

**创建 `layouts/shortcodes/gallery.html`**

```html
{{- $images := split (trim .Inner "\n ") "\n" -}}

<div class="sc-gallery">
  {{ range $images }}
    {{- $src := trim . " \t" -}}
    {{ if $src }}
    <a href="{{ $src }}" class="sc-gallery-item" target="_blank">
      <img src="{{ $src }}" loading="lazy" alt="图片">
    </a>
    {{ end }}
  {{ end }}
</div>
```

**在 `assets/scss/custom.scss` 里添加样式**

```scss
.sc-gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 8px;
  margin: 1.5rem 0;

  .sc-gallery-item {
    display: block;
    overflow: hidden;
    border-radius: 6px;
    aspect-ratio: 16/9;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.3s;

      &:hover {
        transform: scale(1.05);
      }
    }
  }
}
```

> 💡 **Stack 主题自带灯箱效果**，图片链接用 `target="_blank"` 打开原图已足够。如果你想要更精致的灯箱弹窗效果，可以引入 [PhotoSwipe](https://photoswipe.com/) 或 [GLightbox](https://biati-digital.github.io/glightbox/) 库。

---

## 5.5 实战四：Bilibili 视频嵌入（bilibili）

Hugo 内置了 `{{< youtube >}}` 短代码，但没有 Bilibili。国内用户经常需要嵌入 B 站视频，自己实现一个很简单。

**调用方式：**

```
{{</* bilibili BV1xx411c7mD */>}}

{{</* bilibili BV1xx411c7mD p=2 */>}}   ← 指定分P
```

**获取 BV 号的方法：** 打开 B 站视频，地址栏里 `video/` 后面的那串 `BVxxxx` 就是 BV 号。

**创建 `layouts/shortcodes/bilibili.html`**

```html
{{- $bvid := .Get 0 -}}
{{- $page := .Get "p" | default 1 -}}

{{ if $bvid }}
<div class="bilibili-embed">
  <iframe
    src="https://player.bilibili.com/player.html?bvid={{ $bvid }}&page={{ $page }}&high_quality=1&danmaku=0"
    scrolling="no"
    frameborder="0"
    allowfullscreen
    loading="lazy"
  ></iframe>
</div>
{{ end }}
```

**在 `assets/scss/custom.scss` 里添加样式（响应式 16:9）**

```scss
.bilibili-embed {
  position: relative;
  width: 100%;
  padding-top: 56.25%;  // 16:9 比例
  margin: 1.5rem 0;
  border-radius: 8px;
  overflow: hidden;

  iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: none;
  }
}
```

---

## 5.6 Hugo 内置 Shortcode 速查

除了自定义的之外，Hugo 还内置了几个常用 Shortcode，直接用不需要创建文件：

| Shortcode | 用途 | 示例 |
|-----------|------|------|
| `youtube` | 嵌入 YouTube 视频 | `{{< youtube 视频ID >}}` |
| `figure` | 带说明的图片 | `{{< figure src="img.png" caption="图片说明" >}}` |
| `highlight` | 代码高亮（带更多控制） | `{{< highlight python "linenos=true" >}}...{{< /highlight >}}` |
| `ref` | 站内链接 | `[查看这篇文章]({{</* ref "文章文件夹名" */>}})` |
| `relref` | 站内相对链接 | `{{</* relref "文章文件夹名" */>}}` |

---

## 5.7 常见报错与解决

### ❌ 报错一：Shortcode 内容原样输出，没有渲染成 HTML

**原因：** 调用时用了 `%` 包裹而不是 `<>`，或者反过来。

**规则：**
- `{{</* shortcode */>}}`：内容**不**经过 Markdown 渲染，适合纯 HTML 输出的 Shortcode
- `{{%/* shortcode */%}}`：内容会经过 Markdown 渲染，适合包含 Markdown 语法的内容

`notice` 这个 Shortcode 的内容用了 `markdownify` 处理，所以两种都可以。

---

### ❌ 报错二：在文章里展示 Shortcode 语法时，它被自动执行了

**原因：** Hugo 在编译时看到 `{{</*` 就会尝试执行。

**解决：** 在尖括号里加 `/*` 和 `*/` 注释符号：

```
{{</* notice warning */>}}    ← 这样就不会被执行，只显示文本
```

---

### ❌ 报错三：自定义 Shortcode 创建后提示找不到模板

**错误信息：**
```
failed to extract shortcode: template for shortcode "xxx" not found
```

**原因：** 文件路径不对，或者文件名和调用名不一致（区分大小写）。

**检查：**
1. 确认文件放在 `layouts/shortcodes/` 目录（不是 `themes` 里面）
2. 文件名和调用名完全一致：`notice.html` 对应 `{{</* notice */>}}`

---

## 本篇小结

这四个 Shortcode 覆盖了技术博客 90% 的增强需求：

- ✅ `notice`：提示框，标注重要信息和踩坑警告
- ✅ `linkcard`：卡片链接，推荐工具和资源
- ✅ `gallery`：图片画廊，多图整洁展示
- ✅ `bilibili`：B 站视频嵌入，配合 YouTube 双平台覆盖

下一篇我们来讲博客美化进阶——用 `custom.scss` 覆盖 Stack 主题默认样式，调整主题色、字体大小、暗色模式细节，以及如何安全地修改主题而不影响后续升级。

---

## 系列导航

- **上一篇：** [4-Hugo 写文章全攻略：Front Matter、Markdown 语法与封面图](/hugo-writing-guide/)
- **下一篇：** [6-Hugo 美化进阶：custom.scss 覆盖主题样式 + 暗黑模式调优](/hugo-tailwind-dark-mode/)
- **返回系列目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/categories/station/)
