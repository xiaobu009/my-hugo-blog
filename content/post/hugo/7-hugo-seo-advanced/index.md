---
title: "中级05-SEO 进阶——让 Google 和社交平台真正「看懂」你的博客"
date: 2026-07-10
lastmod: 2026-07-10
draft: false
description: "在完成 Search Console 收录的基础上，进一步配置 robots.txt 精细化控制和结构化数据，再补上图片 alt、标题层级、内链这些容易被忽略的细节，让搜索引擎真正读懂你的博客内容。"
keywords: ["Hugo SEO", "robots.txt", "结构化数据", "AI 爬虫屏蔽", "图片alt", "301重定向"]
image: images/hugo-seo-advanced-thumbnail.webp
categories:
  - Hugo建站
tags:
  - Hugo
  - SEO
  - 结构化数据
url: "hugo-seo-advanced"
series: ["Hugo建站指南"]
series_order: 7
---

## 做完这篇你能得到什么

不想被 AI 公司拿去训练模型的内容，可以精确挡掉对应爬虫；文章在 Google 搜索结果里有机会多显示发布日期等额外信息；另外还会把图片 alt、标题结构、内链这些容易被忽略的写作细节一次性捋一遍。

**前提：已经完成 [中级04](https://smallstep.one/hugo-analytics-adsense-basics/) 里 Search Console 的验证和 sitemap 提交——这篇不会重复这两步，直接往后接。如果还没做，先回那篇走一遍。延续中级04的方向，这篇涉及的所有设置同样是针对 Google 搜索引擎做的，不涉及百度等其他搜索引擎。**

------

## 给爬虫定规矩：robots.txt

中级04让 Google 知道了"该去哪儿抓"，这一步反过来——告诉所有爬虫"哪些地方不该抓"。

Hugo 默认是**不**生成 robots.txt 文件的，需要手动开启。打开 `config/_default/hugo.toml`，在文件顶层（不是在 `[params]` 之类的区块里面）加一行：

```toml
enableRobotsTXT = true
```

只加这一行，Hugo 会用内置的默认模板生成一份最基础的 `robots.txt`（基本等于"对谁都不设限"）。如果你只是想让搜索引擎正常抓取，到这一步其实就够了。

但如果你不想让自己写的内容被各家公司拿去训练 AI 模型，可以自己写一份更细的规则。在站点根目录（和 `content/` 同级）新建 `layouts/robots.txt`：

```
User-agent: *
Allow: /

Sitemap: {{ .Site.BaseURL }}sitemap.xml

# 下面这些是各家公司用于 AI 训练的爬虫，按需屏蔽
# 屏蔽这些不影响搜索引擎正常收录你的网站

User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: Bytespider
Disallow: /

User-agent: Applebot-Extended
Disallow: /
```

> **注意区分清楚：** `Google-Extended` 只控制 Google 是否拿你的内容去训练 Gemini 之类的 AI 模型，跟负责把你的网站收进搜索结果的 `Googlebot` 是两个完全不同的爬虫，屏蔽前者**不会**影响你网站在 Google 搜索里的正常收录。这两个名字长得太像，很容易搞混，屏蔽前确认一下名字。

这份文件用到了 Hugo 的模板语法（`{{ .Site.BaseURL }}`），构建时会被替换成你真实的网址，所以文件名后缀还是 `.txt`，内容里可以放模板代码。

本地运行 `hugo server -D`，打开浏览器访问 `http://localhost:1313/robots.txt`，确认内容跟你写的一致——尤其检查最上面的 `User-agent: *` 下面是不是 `Allow: /` 而不是 `Disallow: /`，这个写反是最容易出的事故，一旦写反等于告诉全世界的搜索引擎别来抓你的站。

确认没问题，推送上线：

```
git add .
git commit -m "添加 robots.txt 配置"
git push
```

部署完成后访问 `https://smallstep.one/robots.txt`（换成你自己的域名），能看到刚才写的内容就算成功。

> **另外提一句：** Hugo 会给每个标签、分类自动生成一个列表页，如果某个标签下只有一两篇文章，这种页面内容很薄，Google 可能会判定为低质量页面。现在文章数量还少，不用特别处理；等内容多起来、个别标签页常年只有一篇文章时，可以考虑把 `/tags/` 这类路径也加进上面的 `Disallow` 规则里，不让爬虫抓取这些空壳页面。

------

## 给 Google 一张"身份证"：结构化数据

到这一步，Google 已经知道你的网站在哪儿、能正常抓取了。但 Google 怎么知道这篇文章是谁写的、什么时候发的？这些信息光靠人眼看页面是看不出来的，得靠"结构化数据"明确告诉它。

加了结构化数据之后，文章有机会在 Google 搜索结果里多显示一行发布日期之类的信息，点击率通常会比纯标题链接高一些。这一步偏进阶，不加也完全不影响网站正常运行，有空再做也行。

打开 `myblog/layouts/partials/head/custom.html`（如果这个文件不存在，先在 `layouts/partials/` 下新建 `head` 文件夹，再建这个文件），在已有内容下面加一段：

```html
{{ if .IsPage }}
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": {{ .Title | jsonify }},
  "description": {{ .Description | jsonify }},
  "datePublished": {{ .Date.Format "2006-01-02T15:04:05Z07:00" | jsonify }},
  "dateModified": {{ .Lastmod.Format "2006-01-02T15:04:05Z07:00" | jsonify }},
  "url": {{ .Permalink | jsonify }},
  "author": {
    "@type": "Person",
    "name": "小布"
  }
}
</script>
{{ end }}
```

`{{ if .IsPage }}` 这一行是为了让这段代码只在文章页生效，首页、归档页这些不需要打上"这是一篇文章"的标签。`jsonify` 是 Hugo 自带的函数，负责把标题、描述这些文字安全地转成 JSON 格式——直接拿引号包标题，万一标题里本身带了引号或特殊符号，会把整段 JSON 弄坏，用 `jsonify` 就不用担心这个问题。

`"name": "小布"` 这里换成你自己的署名。

保存，推送：

```
git add .
git commit -m "添加文章结构化数据"
git push
```

------

## 验证 Google 真的看懂了

打开 [Google 富媒体结果测试工具](https://search.google.com/test/rich-results)，输入刚发布过的一篇文章网址，点检测。等几秒，如果显示检测到 `BlogPosting` 这个类型，并且列出了标题、发布日期等字段，说明加对了。

如果提示有错误或警告，点开看具体是哪个字段出的问题，回去检查对应的 Front Matter 是不是缺了 `description` 或者日期格式不对。

------

## 写文章时顺手就该做的几件事

这几件事不涉及配置文件，是写文章过程里养成的习惯，但对 SEO 的影响不比前面那些技术设置小。

**给图片写清楚 alt 文字**

Markdown 插入图片的语法是 `![描述文字](图片路径)`，中间这段"描述文字"就是 alt 文字。很多人图省事直接留空，或者写成 `image1` 这种没有意义的占位词。

alt 文字的作用：一是让图片有机会被 Google 图片搜索收录，二是给视觉障碍读者用的屏幕阅读器提供描述，这是网页无障碍访问的基本要求。写的时候按图片实际内容描述就行，比如"Cloudflare Pages 构建配置页面截图"，不用堆关键词。

**正文标题别跳级**

文章标题（Front Matter 里的 `title`）本身在页面上渲染出来就是 H1，正文里不要再手动打出一个一级标题。正文小标题统一用二级标题（`##`），如果某个步骤下面还要细分，再往下用三级标题（`###`），不要因为想要字号大就跳着用。

本地预览时按 `F12` 打开浏览器开发者工具，在 Elements 面板里搜一下 `<h1>`，一篇文章正常情况下应该只有一个。

**记得加内链**

每篇文章末尾的"系列导航"只覆盖了同系列的前后篇，但写到正文中间提到相关内容时——比如这篇提到了 eSIM、那篇刚好写过 eSIM 测评——应该直接加个链接过去。这个不需要改任何配置，纯粹是写作时养成的习惯：写完一篇文章发布前，回头扫一眼正文，看看有没有提到过自己写过的其他主题，顺手加上链接。

内链做得好，读者会在站内多停留几篇文章，搜索引擎也更容易理解你网站不同文章之间的关联。

------

## 几个用得上再处理的技术点

下面这几件事现在用不上，但提前知道，到时候不会手忙脚乱。

**网站速度**

打开 [PageSpeed Insights](https://pagespeed.web.dev)，输入博客首页或者某篇文章的网址，它会给出评分和具体建议（图片太大、字体加载慢之类）。

Cloudflare 的 CDN 已经帮了大忙，真正的瓶颈通常是图片——截图、配图如果没压缩直接传上去，文件可能几 MB 起步。发布文章前用 [TinyPNG](https://tinypng.com) 之类的工具压缩一下封面图和正文配图，控制在几百 KB 以内，比改任何配置都管用。

**自定义404页面**

Stack 主题自带一份默认的404页面，效果基本够用，不是必须改。如果想自定义文字内容，可以试着在 `content/` 目录下新建 `404.md`；如果改了不生效，说明主题用的是固定模板，需要把主题的 `layouts/404.html` 复制一份到自己站点的 `layouts/` 目录里再改——跟之前改侧边栏小部件遇到的情况是同一个套路。

优先级不高，有空再做。

**以后改 URL，别忘了做重定向**

哪天想给某篇文章换个 url（比如发现关键词不够精准），如果只是改了 Front Matter 里的 `url` 字段直接发布，旧链接会直接变成404——之前 Google 收录的排名、外部平台分享出去的链接，全部断掉。

正确做法是同时设置一个跳转：在 `myblog/static/` 目录下新建一个叫 `_redirects` 的文件（没有后缀名），格式是"旧路径 新路径 状态码"，每条规则一行：

```
/旧的url地址 /新的url地址 301
```

比如把 `/old-slug` 改成了 `/new-slug`：

```
/old-slug /new-slug 301
```

这个文件放在 `static/` 目录下，Hugo 构建时会原样复制到网站根目录，Cloudflare Pages 会自动识别并按这个规则跳转，不需要额外配置。现在用不上，但等真到了要改 URL 那天，记得来翻这一段。

------

## 常见报错与解决

**报错现象：robots.txt 写完之后，Google Search Console 的索引报告显示大量页面被屏蔽**

原因：八成是 `User-agent: *` 下面的规则写反了，把 `Allow: /` 写成了 `Disallow: /`。

解决步骤：打开线上的 `/robots.txt`，逐行检查最顶部那组通用规则，确认是 `Allow: /`；改完重新推送，等 Cloudflare 构建完成后在 Search Console 里用"网址检查"工具重新测一遍这个页面。

**报错现象：加完结构化数据那段代码，网站直接构建失败**

原因：通常是 JSON 格式本身写错了（比如多打了一个逗号、少了一个引号），或者代码片段放的位置不对。

解决步骤：检查路径是不是 `layouts/partials/head/custom.html`（注意是 `partials` 不是 `_partials`，Stack 主题内部用 `_partials`，站点自定义文件放 `partials`）；检查大括号、逗号是不是一一对应，确认每个文字字段都套了 `jsonify` 而不是自己手写引号。

**报错现象：`_redirects` 文件加了规则，但访问旧链接没有跳转，还是显示404**

原因：Cloudflare 对 `_redirects` 里的空格数量很敏感，规则的几部分之间必须用空格或 Tab 分隔，多余的空格可能导致规则被忽略。

解决步骤：打开 Cloudflare Pages 后台对应这次部署的构建日志，看有没有提示某条规则被忽略；本地编辑时用一个 Tab 键代替手动敲多个空格，比较不容易出这种问题。

------

## 这篇做完，你拥有了什么

- 不想被拿去训练 AI 模型的内容，可以通过 robots.txt 挡住对应爬虫，且不影响搜索引擎正常收录
- 文章在 Google 搜索结果里有机会多显示发布日期等额外信息
- 图片 alt、标题结构、内链这些容易被忽略的写作细节捋清楚了
- 网站速度、自定义404、URL 重定向这些以后用得上的技术点，提前心里有数了

这些大多是一次性配置或者写作习惯，做熟了之后基本不用再操心。

下一篇会回到博客本身的功能上，聊聊评论系统、多语言配置和站内搜索优化这几块。

------

**完整 Front Matter 见文章开头，完整访问地址：** https://smallstep.one/hugo-seo-advanced/

**系列导航**

- 上一篇：[中级04——流量与变现基础](/hugo-analytics-adsense-basics/)
- 下一篇：中级06——功能扩展（即将发布）
- 返回系列目录：[Hugo 建站指南](/hugo-guide/)
