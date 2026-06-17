---
title: "让博客更像你的：Stack 主题个性化配置指南"
date: 2026-06-09
lastmod: 2026-06-09
draft: false
description: "博客上线了，但还是默认样子？这篇手把手教你配置头像、昵称、社交媒体链接、侧边栏小部件和导航菜单，做完这篇博客就真的是你的了。"
keywords: ["Hugo", "Stack主题", "个性化配置", "头像设置", "导航菜单", "侧边栏"]
image: images/hugo-stack-config-Thumbnail-3.webp
categories:
  - Hugo建站指南
tags:
  - Hugo
  - Stack主题
  - 个性化配置
series: ["Hugo建站指南"]
series_order: 3
url: "hugo-stack-config"
---

{{< youtube 0MFD1U5tOtU >}}
---

## 做完这篇你能得到什么

博客左侧栏出现你自己的头像和名字，社交媒体图标指向你真实的账号，导航菜单是你定义的内容，右侧小部件显示什么，怎么排序，你来决定。

上一篇部署完之后，打开博客看到的还是 Stack demo 的默认配置——作者叫 John Doe，头像是占位图，社交链接全是示例地址。这篇就是把这些全换成你自己的。

**前提：已完成[初级02——免费把博客发布到全球](https://smallstep.one/hugo-cloudflare-deploy/)，博客已通过 Cloudflare Pages 上线。**

---

## 先认识配置文件

这篇的改动分散在三个文件里，操作之前先知道去哪改：

```
config/_default/
├── params.toml     ← 头像、favicon、侧边栏 emoji、小部件
├── languages.toml  ← 博客名称、侧边栏签名（中文版在这里）
└── menu.toml       ← 社交媒体图标链接
```

导航菜单比较特别，后面第四步单独说。

用 VS Code 打开整个 `myblog` 文件夹，左侧文件树找到这三个文件，后面逐一修改。



---

## 第一步：换上你自己的头像

头像是自己IP的重要标识，也是访客对你博客的第一印象，默认是灰色占位图，这一步换成你自己的图片，顺手把浏览器标签上的小图标也一起换掉。

改动在 `params.toml`，文件顶部找到这两行：

```toml
favicon = "img/avatar.png"

[sidebar]
    avatar = "img/avatar.png"
```

`avatar` 是侧边栏头像，`favicon` 是浏览器标签上的小图标，通常用同一张图就行。两个字段填的都是相对于 `assets/` 目录的路径。

把你的头像图片放到对应位置，再把路径改成你放的位置：

1. 准备一张正方形头像图，尺寸 150×150px 以上
2. 放到 `myblog/assets/img/` 下，文件名随意，比如 `avatar.png`
3. 路径和文件名对应就行，放了 `assets/img/avatar.jpg` 就填 `img/avatar.jpg`

> **注意：** 头像必须放在 `assets/` 目录下，不能放 `static/`。Stack 4.0 的头像走 Hugo Pipes 图片处理，只读 `assets/` 里的文件，放错地方会显示破图。

> **验证：** 运行 `hugo server -D`，打开 `http://localhost:1313`，左侧栏出现你的头像。显示破图就检查路径大小写是否完全一致。

---

## 第二步：改博客名称和侧边栏签名

博客名称和签名是你告诉访客「这是谁的博客、写什么内容」的地方，现在还是 demo 的默认文字。

**博客名称**显示在侧边栏头像下方，同时也出现在浏览器标签和页面顶部。在 `languages.toml` 里找到 `[zh]` 区块修改：

```toml
[zh]
    label  = "简体中文"
    title  = "你的博客名称"      ← 改这里
    weight = 2
```

**侧边栏签名**显示在博客名称下方，一句话说清楚博客方向或者自我介绍。同样在 `languages.toml` 的 `[zh.params.sidebar]` 里改：

```toml
    [zh.params.sidebar]
        subtitle = "你的一句话签名"   ← 改这里
```

> **为什么不在 `params.toml` 里改？** `params.toml` 里的 `[sidebar]` 下也有一个 `subtitle` 字段，但 Stack 4.0 多语言架构下，中文版的签名会被 `languages.toml` 里的值覆盖，改 `params.toml` 不会生效。

**顺带：侧边栏 emoji** 也在 `params.toml` 的 `[sidebar]` 里，显示在博客名称旁边，改成你喜欢的或者删掉这行不显示：

```toml
[sidebar]
    emoji = "✏️"
```

> **验证：** 保存后刷新本地预览，左侧栏头像下方出现你的博客名称和签名。

---

## 第三步：配置社交媒体链接

社交链接让访客知道去哪找你，现在指向的是 demo 的示例地址。这一步把它们换成你自己的账号。

改动在 `menu.toml`，每一段 `[[social]]` 对应一个图标：

```toml
[[social]]
    identifier = "github"
    name       = "GitHub"
    url        = "https://github.com/CaiJimmy/hugo-theme-stack"   ← 换成你的

    [social.params]
        icon = "brand-github"
```

**修改现有链接：** 把 `url` 换成你自己的账号地址，`name` 是鼠标悬停时显示的文字，按需修改。

**删除不需要的平台：** 把整段 `[[social]]` 连同下面的 `[social.params]` 一起删掉。

**添加新平台：** 复制任意一段，修改 `identifier`（唯一标识，不重复即可）、`name`、`url` 和 `icon` 四个字段。

**icon图标说明：**`icon` 的值来自 [Tabler Icons](https://tabler.io/icons)，在网站里搜索图标名，下载使用即可。图标默认在 stack主题模板\assets\icons 目录下，下载的icon文件可以放在这里，也可以放到自己站点的\assets\icons 目录下，Stack 主题加载图标时会优先读取站点自己的 `assets/icons/`，找不到才去主题目录里找。这是 Hugo 的标准覆盖机制。建议优先放到自己的 `assets/icons/`目录下。

常用平台图标名参考：

| 平台 | icon 值 |
|------|---------|
| GitHub | `brand-github` |
| Twitter / X | `brand-x` |
| YouTube | `brand-youtube` |
| 微博 | `brand-weibo` |
| 邮件 | `mail` |
| RSS | `rss` |

> **验证：** 保存后刷新本地预览，左侧栏头像下方出现对应图标，点击能跳转到正确链接。

---

## 第四步：调整侧边栏小部件

小部件控制侧边栏显示哪些内容模块——搜索框、归档列表、分类、标签云都在这里管。默认全部开启，你可以按需裁剪或调整顺序。

改动在 `params.toml` 的 `[widgets]` 区块：

```toml
[widgets]
    homepage = [
        { type = "search" },
        { type = "archives", params = { limit = 5 } },
        { type = "categories", params = { limit = 10 } },
        { type = "tag-cloud", params = { limit = 10 } },
    ]
    page = [{ type = "toc" }]
```

`homepage` 控制首页侧边栏，`page` 控制文章页侧边栏（默认只有目录，保持不动）。

**调整显示顺序：** 改变数组里的排列顺序就是改变显示顺序。

**关闭某个小部件：** 删掉对应那一行。

**调整显示数量：** 修改 `limit` 后面的数字。

比如只保留搜索框和归档，去掉分类和标签云：

```toml
[widgets]
    homepage = [
        { type = "search" },
        { type = "archives", params = { limit = 5 } },
    ]
    page = [{ type = "toc" }]
```

> **验证：** 保存后重启本地服务（`Ctrl+C` 后重新运行 `hugo server -D`），首页侧边栏按你设置的内容显示。

---

## 第五步：调整导航菜单

导航菜单是访客在博客里跳转的路标，默认是英文的，顺序也是 demo 定的。这一步改成中文，顺序也按你的习惯来。

Stack 4.0 的导航菜单不在 `menu.toml` 里集中管理，而是由每个固定页面自己决定要不要出现在菜单里。初级02 从 demo 复制来的四个固定页面，各自的 Front Matter 里都有这样一段：

```yaml
menu:
    main:
        weight: -90       ← 值越小越靠前
        params:
            icon: user    ← 菜单图标
```

打开 `content/page/` 下的各个页面文件修改：

```
content/page/
├── about/index.zh.md     ← 关于页
├── archives/index.zh.md  ← 归档页
├── links/index.zh.md     ← 友情链接页
└── search/index.zh.md    ← 搜索页
```

**修改显示名称：** 改 Front Matter 里的 `title` 字段，即导航菜单名称。

> **顺带说一下 `description` 字段：** 每个固定页面的 Front Matter 里都有 `description` 字段，它不显示在页面正文里，而是输出到页面 `<head>` 的 `<meta name="description">` 标签供搜索引擎抓取，同时也作为卡片摘要显示在列表页。写给搜索引擎看的，不是给访客看的正文内容。

**调整顺序：** 修改 `weight` 值，数字越小越靠前。

**修改图标：** 改 `icon` 字段，同样来自 Tabler Icons。常用图标：

| 页面 | 推荐 icon |
|------|-----------|
| 关于 | `user` |
| 归档 | `archive` |
| 友情链接 | `link` |
| 搜索 | `search` |

**添加新菜单项：** 新建一个固定页面，在它的 Front Matter 里加上 `menu.main` 这段，它就自动出现在导航栏里。

> **不要删除 `archives` 和 `search` 这两个页面。** 侧边栏的归档小部件和搜索小部件点击后会跳转到这两个页面，删了会 404。

> **验证：** 保存后刷新，顶部导航栏显示更新后的名称，点击各项能正常跳转。

---

## 第六步：推送上线

本地预览确认没问题后，推送到 GitHub，Cloudflare 自动重新构建：

```bash
git add .
git commit -m "个性化配置：头像、社交链接、导航菜单"
git push
```

等待 1-3 分钟，打开你的博客域名，确认所有修改已生效。

---

## 常见问题

**Q：头像显示破图**

A：检查两点：一是图片是否放在 `myblog/assets/` 目录下（不是 `static/`）；二是 `params.toml` 里 `avatar` 的路径和文件名是否和实际文件完全一致，包括大小写。

**Q：改了 `params.toml` 里的 `subtitle`，但签名没变**

A：中文版的签名要在 `languages.toml` 的 `[zh.params.sidebar]` 里改，`params.toml` 里的 `subtitle` 对中文版无效。

**Q：社交图标不显示或显示为方块**

A：`icon` 字段填错了。去 [tabler.io/icons](https://tabler.io/icons) 搜索图标名，确认拼写正确，不要加 `tabler-` 前缀。

**Q：改了小部件配置但页面没变化**

A：停止本地服务（`Ctrl+C`），重新运行 `hugo server -D`，配置文件的改动有时需要重启才能生效。

**Q：`[widgets]` 删完小部件后 Hugo 报错**

A：检查最后一行末尾是否多了逗号，删掉即可。

---

## 做完这篇，你的博客有了什么

- 左侧栏显示你自己的头像、博客名称和签名，不再是灰色占位图和 Lorem ipsum
- 社交媒体图标指向你真实的账号
- 侧边栏小部件按你的需求排列
- 导航菜单是中文，结构是你定义的

---

## 系列导航

- 上一篇：[初级02——免费把博客发布到全球：GitHub + Cloudflare Pages 部署完整指南](https://smallstep.one/hugo-cloudflare-deploy/)
- 下一篇：中级02——写作体验优化：Front Matter、文章模板与封面图规范（即将发布）
- 返回系列目录：[Hugo 建站完全指南](https://smallstep.one/hugo-guide/)

---

## Front Matter

```yaml
---
title: "让博客更像你的：Stack 主题个性化配置指南"
date: 2026-06-09
lastmod: 2026-06-09
draft: false
description: "博客上线了，但还是默认样子？这篇手把手教你配置头像、昵称、社交媒体链接、侧边栏小部件和导航菜单，做完这篇博客就真的是你的了。"
keywords: ["Hugo", "Stack主题", "个性化配置", "头像设置", "导航菜单", "侧边栏"]
categories:
  - Hugo建站指南
tags:
  - Hugo
  - Stack主题
  - 个性化配置
series: ["Hugo建站指南"]
series_order: 3
url: "hugo-stack-config"
---
```

