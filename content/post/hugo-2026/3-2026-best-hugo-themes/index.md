---
title: "3-Stack 主题安装与基础配置完整教程（2026 最新）"
date: 2026-03-27T09:37:23+08:00
lastmod: 2026-05-17
draft: false
description: "手把手教你安装 Hugo Stack 主题，配置侧边栏、导航菜单、头像、中文语言包，并创建归档、搜索、关于等固定页面。包含 exampleSite 快速上手方案和 4 大常见报错解决。"
keywords: ["Hugo Stack 主题安装", "hugo-theme-stack 配置", "Hugo Stack 侧边栏", "Hugo Stack 中文", "Hugo Stack 归档页", "hugo.toml 配置"]
url: "2026-best-hugo-themes"
categories:
    - station
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

上一篇我们装好了 Hugo，创建了第一个站点，但页面还是光秃秃的。这一篇来安装 Stack 主题——装完之后，你的博客就会有完整的三栏布局、暗色模式、文章分类、标签云、搜索等功能，颜值直接拉满。

**为什么选 Stack 主题？**

Hugo 官方主题库有几百个主题，我用 Stack 的理由很简单：

- 三栏布局（左侧边栏 + 文章列表 + 右侧小部件），信息密度高，适合内容博客
- 原生支持暗色模式，且切换逻辑干净
- 内置归档、搜索、分类、标签云等功能，不需要额外插件
- 维护活跃，GitHub 持续更新，社区问题多数能找到解答
- 对中文支持友好，有完整的 `zh-cn` 语言包

---

## 3.1 安装 Stack 主题

安装主题有两种方式，我们选择更适合深度自定义的 **git clone 方式**。

在博客根目录（如 `D:\web\my-blog\`）打开命令行，执行：

```bash
git clone https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```

下载完成后，`themes/hugo-theme-stack/` 目录里就是完整的主题文件。

> 💡 **网络问题怎么办？** 如果 GitHub 下载缓慢，把命令里的地址改成镜像：
> ```bash
> git clone https://mirror.ghproxy.com/https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
> ```

> ⚠️ **关于 .git 文件夹：** git clone 会在主题目录里留下一个 `.git` 隐藏文件夹。**现阶段不用管它**，等到第 7 篇部署到 Cloudflare 之前再处理。

---

## 3.2 用 exampleSite 快速上手（强烈推荐）

Stack 主题自带了一个完整的示例站点（`exampleSite`），里面有配置好的 `hugo.toml`、示例文章、固定页面等。**直接把它复制过来用，比从零配置省 90% 的时间。**

**第一步：复制配置文件**

把主题示例站点的配置文件复制到博客根目录，**替换**原有的 `hugo.toml`：

```bash
# 复制配置文件（Windows 命令行）
copy themes\hugo-theme-stack\exampleSite\hugo.toml hugo.toml
```

> ⚠️ 如果你的根目录是 `config.yaml` 而不是 `hugo.toml`，先把它删掉，Hugo 同时存在两个配置文件会报错。

**第二步：复制示例内容**

把示例站点的 `content` 目录整个复制到博客根目录：

```bash
# 复制整个 content 目录
xcopy themes\hugo-theme-stack\exampleSite\content content /E /I /Y
```

这里面包含：
- `content/page/about/`：关于我页面
- `content/page/archives/`：归档页面
- `content/page/search/`：搜索页面
- `content/post/`：示例文章（可以全部删掉换成自己的）

**第三步：在配置文件里指定主题**

打开根目录的 `hugo.toml`，找到 `theme` 这一行，确认值是：

```toml
theme = "hugo-theme-stack"
```

**第四步：本地预览**

```bash
hugo server -D
```

打开浏览器访问 `http://localhost:1313/`，你应该能看到一个有完整三栏布局的博客了。

---

## 3.3 核心配置详解

exampleSite 的配置文件是英文且包含多语言设置，下面帮你逐项讲清楚需要修改的地方。

用 VS Code 打开根目录的 `hugo.toml`，按以下说明修改：

### 基础信息

```toml
baseURL = "https://你的域名.com/"   # 本地开发期间改成 http://localhost:1313/
                                    # 部署后再改成真实域名
languageCode = "zh-cn"
hasCJKLanguage = true               # 中文字数统计必须加
title = "你的博客名称"
```

### 分页设置

```toml
paginate = 5    # 首页每页显示几篇文章，建议 5-10
```

### 固定链接格式

```toml
[permalinks]
  [permalinks.page]
    page = "/:slug/"
  [permalinks.post]
    post = "/:slug/"
```

这样文章 URL 会是 `你的域名/文章slug/` 的格式，简洁且对 SEO 友好。

### 语言配置（重点）

exampleSite 默认是多语言配置（中英文+阿拉伯文），我们只保留中文：

```toml
DefaultContentLanguage = "zh-cn"

[languages]
  [languages.zh-cn]
    languageName = "中文"
    title = "你的博客名称"
    weight = 1
    [languages.zh-cn.params]
      description = "你的博客简介"
```

### 侧边栏配置

```toml
[params]
  [params.sidebar]
    emoji = "🍀"                    # 头像旁边的 emoji，随便改成你喜欢的
    subtitle = "你的博客 Slogan"    # 头像下方的一句话介绍
    [params.sidebar.avatar]
      enabled = true
      local = true                  # true = 使用本地图片
      src = "img/avatar.png"        # 头像图片路径（放在 static/img/ 目录下）
```

**头像图片怎么放？**

在站点根目录的 `static/` 文件夹里新建一个 `img/` 文件夹，把你的头像图片命名为 `avatar.png` 放进去：

```
static/
└── img/
    └── avatar.png    ← 头像图片放这里
```

### 文章页配置

```toml
[params.article]
  math = false          # 是否启用数学公式渲染，写技术博客一般不需要
  toc = true            # 是否显示文章目录（TOC），长文强烈建议开启
  readingTime = true    # 是否显示阅读时长
  [params.article.license]
    enabled = false     # 是否在文章底部显示版权声明
```

### 日期格式

```toml
[params.dateFormat]
  published = "2006-01-02"          # 发布日期格式
  lastUpdated = "2006-01-02 15:04"  # 最后更新时间格式
```

> 💡 **注意：** Hugo 的日期格式用的是 Go 语言的基准时间 `2006-01-02 15:04:05`，这几个数字是固定的，不能改成其他数字，否则日期会显示错误。

---

## 3.4 配置导航菜单

Stack 主题的导航菜单通过 `hugo.toml` 的 `[menu]` 配置，或者直接在 `content/page/` 目录下的页面 Front Matter 里设置。

**方式一：在 Front Matter 里设置（推荐）**

打开 `content/page/archives/index.zh-cn.md`，会看到类似：

```yaml
---
title: "归档"
links:
menu:
    main:
        weight: -90
        params:
            icon: archives
---
```

`menu.main.weight` 控制菜单顺序，数字越小越靠前（负数靠最前）。

Stack 主题内置的菜单图标（`params.icon`）可用值：

| 图标名 | 含义 |
|--------|------|
| `home` | 主页 |
| `archives` | 归档 |
| `search` | 搜索 |
| `link` | 友链 |
| `user` | 关于 |

**方式二：在 hugo.toml 里直接配置**

```toml
[[menu.main]]
    identifier = "home"
    name = "主页"
    url = "/"
    weight = -100
    [menu.main.params]
        icon = "home"

[[menu.main]]
    identifier = "archives"
    name = "归档"
    url = "/archives/"
    weight = -90
    [menu.main.params]
        icon = "archives"

[[menu.main]]
    identifier = "search"
    name = "搜索"
    url = "/search/"
    weight = -80
    [menu.main.params]
        icon = "search"
```

---

## 3.5 创建必要的固定页面

如果你直接复制了 exampleSite 的 content，这些页面已经有了。如果没有，手动创建：

**归档页**（必须有，否则归档菜单点开是 404）：

新建 `content/page/archives/index.zh-cn.md`：

```yaml
---
title: "归档"
layout: "archives"
slug: "archives"
menu:
    main:
        weight: -90
        params:
            icon: archives
---
```

**搜索页**（Stack 主题内置搜索，必须创建此页面才能启用）：

新建 `content/page/search/index.zh-cn.md`：

```yaml
---
title: "搜索"
layout: "search"
slug: "search"
menu:
    main:
        weight: -70
        params:
            icon: search
---
```

**关于页**：

新建 `content/page/about/index.zh-cn.md`：

```yaml
---
title: "关于"
slug: "about"
menu:
    main:
        weight: -60
        params:
            icon: user
---

你好，我是小布，这里写你的自我介绍...
```

---

## 3.6 右侧边栏小部件配置

Stack 主题右侧可以配置多个小部件（Widget），在 `hugo.toml` 里设置：

```toml
[params.widgets]
    homepage = [
        { type = "search" },
        { type = "archives", params = { limit = 5 } },
        { type = "categories", params = { limit = 10 } },
        { type = "tag-cloud", params = { limit = 20 } },
    ]
    page = [
        { type = "toc" },
    ]
```

- `homepage`：首页右侧显示的小部件列表
- `page`：文章页右侧显示的小部件（`toc` = 文章目录）
- `limit`：各类别/标签最多显示几个

---

## 3.7 favicon 配置

在 `static/` 目录下放一个 `favicon.ico` 文件，然后在 `hugo.toml` 里指定：

```toml
[params]
    favicon = "/favicon.ico"
```

没有 favicon 图片？推荐用 [favicon.io](https://favicon.io/) 免费生成，支持从文字或图片一键生成。

---

## 3.8 常见报错与解决方案

### ❌ 报错一：首页空白，或提示 "page not found"

**原因：** `hugo.toml` 里没有指定 `theme`，或者主题文件夹名称和配置里不一致。

**解决：** 确认以下两点：
1. 主题文件夹实际存在于 `themes/hugo-theme-stack/`
2. `hugo.toml` 里有 `theme = "hugo-theme-stack"`（名称完全一致，区分大小写）

---

### ❌ 报错二：TOCSS 报错，页面无样式

**错误信息：**
```
ERROR TOCSS: failed to transform "scss/style.scss"
```

**原因：** 安装的是普通版 Hugo，不是 Extended 版，无法编译 SCSS。

**解决：** 回到第 2 篇，下载并替换为 `hugo_extended_xxx_windows-amd64.zip` 版本。

---

### ❌ 报错三：归档/搜索页面点击后 404

**原因：** 对应的固定页面文件不存在，或者文件名格式不对。

**解决：** 按 3.5 节的说明，确认 `content/page/archives/` 和 `content/page/search/` 目录下有对应的 `.md` 文件，且文件的 `layout` 字段值正确（`archives` 和 `search`）。

---

### ❌ 报错四：多语言配置警告

**警告信息：**
```
WARN languages.zh-cn.description: custom params on the language top level is deprecated
```

**原因：** Hugo 较新版本要求语言参数放在 `[languages.zh-cn.params]` 下，而不是 `[languages.zh-cn]` 顶层。

**解决：** 把 `description` 移到 `[languages.zh-cn.params]` 块里：

```toml
[languages.zh-cn.params]
    description = "你的博客简介"   # ← 放在这里，不是 [languages.zh-cn] 直接下面
```

---

## 本篇小结

到这里，你已经拥有了一个有完整外观的 Hugo 博客：

- ✅ Stack 主题安装完成
- ✅ 基础配置（语言、标题、侧边栏、头像）设置好
- ✅ 导航菜单配置完成
- ✅ 归档、搜索、关于页面创建完成

下一篇我们来学习怎么写文章——Front Matter 每个字段的含义、Markdown 常用语法、封面图片怎么加、文章分类和标签怎么用，以及怎么自定义文章模板让每次新建文章自动带好所有字段。

---

## 系列导航

- **上一篇：** [2-Hugo v0.158 本地安装 + 创建第一个站点](/hugo-v0158-local-install/)
- **下一篇：** [4-写文章全攻略：Front Matter、Markdown 语法与封面图](/hugo-writing-guide/)
- **返回系列目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/categories/station/)
