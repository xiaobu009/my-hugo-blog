---
title: "4-Hugo 写文章全攻略：Front Matter、Markdown 语法、封面图与文章模板"
date: 2026-03-28
lastmod: 2026-05-17
draft: false
description: "全面讲解 Hugo Stack 主题下的文章写作流程：Front Matter 每个字段详解、Markdown 常用语法速查、封面图添加方法、分类与标签使用，以及自定义 archetypes 模板实现一键创建带完整字段的文章。"
keywords: ["Hugo Front Matter", "Hugo 写文章", "Hugo Markdown", "Hugo 封面图", "Hugo archetypes", "Hugo Stack 文章", "hugo new content"]
url: "hugo-writing-guide"
categories:
    - station
tags:
    - Hugo
    - 静态站点
    - 建站教程
---


主题装好了，接下来就是博客最核心的事情——**写文章**。

Hugo 的文章是 Markdown 格式的纯文本文件，在文件最顶部有一块特殊的配置区域叫做 **Front Matter（前置元数据）**，用来告诉 Hugo 这篇文章的标题、日期、分类、封面图等信息。

理解 Front Matter 是写好 Hugo 文章的关键，这篇教程把所有常用字段都讲清楚，并在最后教你自定义文章模板，以后每次新建文章自动带好所有字段。

---

## 4.1 文章的目录结构

Hugo Stack 主题推荐用"页面包（Page Bundle）"方式组织文章——**每篇文章一个独立文件夹**，文章本身是文件夹里的 `index.md`，文章用到的图片也放在同一个文件夹里。

```
content/
└── post/
    ├── my-first-post/          ← 文章文件夹（用英文，作为 URL slug）
    │   ├── index.md            ← 文章正文
    │   ├── cover.jpg           ← 封面图（和文章放在一起）
    │   └── screenshot.png      ← 文章内引用的截图
    └── hugo-deploy-guide/
        ├── index.md
        └── cover.jpg
```

**为什么推荐这种结构？**

- 文章和它的配图放在一起，搬运文章时不会丢失图片
- URL 直接用文件夹名，简洁且语义化（`你的域名/my-first-post/`）
- 支持在文章里用相对路径引用图片（`![](cover.jpg)`），不需要写完整路径

**新建文章的命令：**

```bash
hugo new content post/文章英文名/index.md
```

例如：

```bash
hugo new content post/hugo-deploy-guide/index.md
```

Hugo 会自动在 `content/post/hugo-deploy-guide/` 目录下创建 `index.md`，并套用 archetypes 模板（后面会讲怎么自定义模板）。

---

## 4.2 Front Matter 完整字段详解

Front Matter 是文章文件最顶部两行 `---` 之间的内容，使用 YAML 格式。下面是 Hugo Stack 主题下常用的完整字段说明：

```yaml
---
# ===== 基础信息 =====
title: "文章标题"                      # 必填，显示在文章头部和浏览器标签
date: 2026-05-17T10:00:00+08:00       # 必填，发布日期，影响文章排序
lastmod: 2026-05-17T15:30:00+08:00    # 最后修改时间，显示在文章底部
draft: false                           # false=正式发布，true=草稿（不显示）

# ===== SEO 相关 =====
description: "这篇文章的简短描述"      # 显示在文章列表摘要和搜索引擎描述
keywords: ["关键词1", "关键词2"]       # SEO 关键词，帮助搜索引擎理解文章内容
slug: "custom-url-slug"               # 自定义 URL，不填则用文件夹名作为 URL

# ===== 分类与标签 =====
categories:
    - 技术分享                         # 文章所属分类（可多个，建议 1-2 个）
tags:
    - Hugo                             # 文章标签（可多个，建议 3-5 个）
    - 建站教程

# ===== 封面图 =====
image: "cover.jpg"                    # 封面图文件名，图片和 index.md 放在同一文件夹

# ===== Stack 主题特有字段 =====
featured: true                        # true=加入精选文章列表（侧边栏精选 Widget）
weight: 1                             # 文章排序权重，数字越小越靠前（用于置顶）
comments: true                        # 是否显示评论区（需要配置评论系统）
math: false                           # 是否启用数学公式（KaTeX）

# ===== 文章链接（用于侧边栏链接小部件）=====
links:
    - title: "相关项目名称"
      website: "https://example.com"
      image: "https://example.com/icon.png"
---
```

**关于 `date` 格式：** Hugo 支持多种日期格式，推荐使用带时区的完整格式 `2026-05-17T10:00:00+08:00`（`+08:00` 是北京时间），这样文章排序和"最近发布"功能才准确。

---

## 4.3 分类（categories）与标签（tags）的使用原则

很多人搞不清 categories 和 tags 的区别，这里说清楚：

**分类（categories）**：大的内容方向，像书的章节，一篇文章通常只属于 1-2 个分类。

```yaml
categories:
    - 技术分享
    - Hugo建站
```

**标签（tags）**：具体的知识点或关键词，一篇文章可以有多个标签。

```yaml
tags:
    - Hugo
    - Cloudflare
    - 静态站点
    - 建站教程
```

**Stack 主题如何展示分类和标签：**

- 左侧边栏显示分类列表（各分类文章数量）
- 右侧边栏显示标签云（标签字体大小根据使用频率自动调整）
- 每个分类和标签都有独立的聚合页面，访客可以点击筛选

**为分类添加封面图：**

在 `content/categories/分类名/` 目录下创建 `_index.md`，放一张 `cover.jpg`：

```
content/
└── categories/
    └── 技术分享/
        ├── _index.md       ← 分类描述
        └── cover.jpg       ← 分类封面图
```

`_index.md` 内容：

```yaml
---
title: "技术分享"
description: "折腾过的技术记录"
image: "cover.jpg"
---
```

---

## 4.4 封面图（Featured Image）完整指南

封面图对文章的点击率影响很大，有封面图的文章在列表页视觉上明显更吸引人。

### 添加封面图

在文章文件夹里放一张图片，在 Front Matter 里用 `image` 字段指定：

```yaml
image: "cover.jpg"
```

图片和 `index.md` 在同一个文件夹里：

```
content/post/my-article/
├── index.md
└── cover.jpg    ← 封面图
```

### 封面图尺寸建议

| 用途 | 推荐尺寸 | 说明 |
|------|----------|------|
| 文章封面 | 1200×630px | 符合 Open Graph 标准，分享到社交媒体时显示完整 |
| 分类封面 | 800×600px | 在分类卡片里显示 |
| 头像 | 400×400px | 正方形，Stack 主题会自动裁成圆形 |

### 图片格式建议

优先使用 **WebP** 格式，同等质量下文件体积比 JPG 小约 30%，页面加载更快。Hugo v0.158 支持直接处理 WebP 格式，不需要额外工具。

如果没有现成的封面图，可以用：
- [Unsplash](https://unsplash.com/)：免费高质量图片，可商用
- [Canva](https://www.canva.com/)：在线设计封面图，有大量模板

### 在文章正文里插入图片

图片和 `index.md` 在同一文件夹时，用相对路径引用：

```markdown
![图片描述](screenshot.png)

<!-- 指定宽度 -->
![图片描述](screenshot.png)
```

Stack 主题会自动给图片加上灯箱效果（点击放大），不需要额外配置。

---

## 4.5 Markdown 常用语法速查

Hugo 使用标准 Markdown 语法，下面是写技术博客最常用的部分：

### 标题

```markdown
# 一级标题（文章里不用，Front Matter 里的 title 就是一级标题）
## 二级标题
### 三级标题
#### 四级标题
```

### 文字格式

```markdown
**粗体文字**
*斜体文字*
~~删除线~~
`行内代码`
```

### 代码块

````markdown
```bash
hugo server -D
```

```python
print("Hello, Hugo!")
```

```toml
baseURL = "https://example.com"
```
````

代码块支持语法高亮，在开头的三个反引号后指定语言名称即可（`bash`、`python`、`toml`、`yaml`、`html`、`css`、`javascript` 等）。

### 链接与图片

```markdown
[链接文字](https://example.com)
[链接文字](https://example.com "鼠标悬停提示")

![图片描述](image.png)
![图片描述](https://example.com/image.png)
```

### 列表

```markdown
- 无序列表项一
- 无序列表项二
  - 嵌套列表项

1. 有序列表项一
2. 有序列表项二
```

### 引用块

```markdown
> 这是一段引用文字，适合引用别人说的话或者做重点提示。
>
> 多行引用继续用 > 开头。
```

### 表格

```markdown
| 列名一 | 列名二 | 列名三 |
|--------|--------|--------|
| 内容   | 内容   | 内容   |
| 内容   | 内容   | 内容   |
```

### 分割线

```markdown
---
```

### 任务列表（Stack 主题支持）

```markdown
- [x] 已完成的任务
- [ ] 未完成的任务
```

---

## 4.6 Stack 主题特有的 Shortcodes

Shortcodes 是 Hugo 的内容增强语法，Stack 主题内置了几个实用的：

### 嵌入 YouTube 视频

```
{{< youtube 视频ID >}}
```

视频 ID 是 YouTube 链接里 `v=` 后面的那串字符，例如链接 `https://www.youtube.com/watch?v=dQw4w9WgXcQ`，视频 ID 就是 `dQw4w9WgXcQ`。

### 图片画廊（多图展示）

```
{{</* gallery */>}}
  {{</* figure src="image1.jpg" caption="图片一" */>}}
  {{</* figure src="image2.jpg" caption="图片二" */>}}
{{</* /gallery */>}}
```

### 提示框（Notice Box）

```
{{</* tice info */>}}
这是一个信息提示框，适合补充说明。
{{</* otice */>}}

{{</* tice warning */>}}
这是一个警告提示框，适合标注重要注意事项。
{{</* otice */>}}
```

---

## 4.7 自定义 archetypes 文章模板

每次用 `hugo new content` 新建文章时，Hugo 会套用 `archetypes/default.md` 里的模板。默认模板只有 title、date、draft 三行，每次都要手动补充其他字段，很麻烦。

自定义一个完整的模板，以后新建文章自动带好所有字段，省去大量重复工作。

打开（或创建）`archetypes/default.md`，替换成以下内容：

```markdown
---
title: "{{ replace .File.ContentBaseName `-` ` ` | title }}"
date: {{ .Date }}
lastmod: {{ .Date }}
draft: false
description: ""
keywords: []
categories:
    - 未分类
tags:
    - 标签
image: "cover.jpg"
slug: "{{ .File.ContentBaseName }}"
---

在这里写文章正文...
```

**模板变量说明：**

- `{{ replace .File.ContentBaseName `-` ` ` | title }}`：自动把文件夹名里的连字符换成空格，并首字母大写，作为默认标题
- `{{ .Date }}`：自动填入当前时间
- `{{ .File.ContentBaseName }}`：文件夹名，用作 URL slug

**同时为 Stack 主题创建专用模板：**

在 `archetypes/` 目录下创建 `post.md`（对应 `content/post/` 目录下的文章），内容参考项目文件 `3-Hugo-Front Matter参考.txt` 中的格式：

```markdown
---
title: ""
date: {{ .Date }}
lastmod: {{ .Date }}
draft: false
description: ""
keywords: []
url: ""
categories:
    - 技术分享
tags:
    - Hugo
image: "cover.jpg"
---

```

这样执行 `hugo new content post/文章名/index.md` 时，会优先使用 `archetypes/post.md` 模板（Hugo 会根据内容路径匹配最精确的模板）。

---

## 4.8 文章发布前的检查清单

每次写完文章准备发布时，对照这个清单检查：

```
✅ title：填写了清晰、有关键词的标题
✅ date：日期格式正确，不是未来日期
✅ draft：已改为 false
✅ description：填写了 100-160 字符的文章描述（SEO 重要）
✅ categories：设置了正确的分类
✅ tags：添加了 3-5 个相关标签
✅ image：封面图文件已放入文章文件夹，路径填写正确
✅ 文章内容：标题结构合理（用 ##、### 而非 #）
✅ 代码块：指定了语言名称（有语法高亮）
✅ 图片：所有图片都有 alt 描述文字
✅ 本地预览：hugo server 看过没有渲染错误
```

---

## 本篇小结

到这里你已经掌握了 Hugo 写文章的完整工作流：

- ✅ 理解了文章的目录结构（页面包方式）
- ✅ Front Matter 所有常用字段都清楚了
- ✅ 知道怎么加封面图、设置分类和标签
- ✅ 掌握了 Markdown 常用语法和 Stack 特有 Shortcodes
- ✅ 配置好了自定义文章模板，新建文章自动带好所有字段

下一篇我们来讲 Hugo 的 **Shortcodes 进阶与自定义**——怎么写自己的 Shortcode，实现一些 Markdown 原生做不到的效果，比如带颜色的提示框、卡片式链接、折叠内容等。

---

## 系列导航

- **上一篇：** [3-Stack 主题安装与基础配置完整教程](/2026-best-hugo-themes/)
- **下一篇：** [5-Hugo Shortcodes 进阶：自定义短代码实战](/hugo-custom-shortcode/)
- **返回系列目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/categories/station/)
