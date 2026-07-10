---
title: "中级04-流量与变现基础——Google Analytics、Search Console、AdSense 起步指南"
date: 2026-07-01
lastmod: 2026-07-01
draft: false
description: "用最少的步骤把 Google Analytics 4、Search Console、AdSense 这三个工具的角色和起步方式说清楚，让你知道博客上线之后下一步该做什么。"
keywords: ["Hugo", "Google Analytics", "GA4", "Search Console", "AdSense", "博客变现", "SEO"]
image: images/hugo-analytics-adsense-basics-thumbnail-1.webp
categories:
  - Hugo建站
tags:
  - Hugo
url: "hugo-analytics-adsense-basics"
series: ["Hugo建站指南"]
series_order: 6
---

## 做完这篇你能得到什么

博客上线后，下一步该做什么？本篇用最清晰的方式，帮你理顺 Google Analytics 4、Search Console 和 AdSense 三个核心工具的角色与起步流程。先看数据、再让 Google 收录、最后申请广告变现，一步步打好基础，避免新手常见的混淆和弯路。

> **写在前面：** 这篇以及后面中级05涉及的 Search Console、robots.txt、结构化数据这些设置，都是针对 **Google 搜索引擎** 做的。如果你的读者主要从百度搜过来，这套配置不会有直接效果——百度有自己独立的一套站长平台和规则，不在这个系列的覆盖范围内。

------

## 先把这三个工具捋清楚

新手最容易把这三件事混在一起，结果装了统计工具就以为博客自动会被搜到，或者刚上线两篇文章就急着去申请广告，被拒了还摸不着头脑。先拆开说：

- **Google Analytics（GA4）**：告诉你"谁来看了你的博客"——多少人访问、从哪来、停留多久、看了哪些文章。它只负责统计，跟博客是否被搜索引擎收录没有关系。
- **Google Search Console（GSC）**：告诉 Google"我的博客在这里"，让搜索引擎能正常抓取和收录你的页面，也能看到读者用什么关键词搜到你。装了 GA 不代表 Google 已经知道你的博客存在，这是两件独立的事。
- **Google AdSense**：在博客上挂广告，靠展示和点击赚钱，是变现工具，跟前两个没有直接关系，但通常需要先有一定的内容量、并且处于被收录状态，才有资格申请。

对应的顺序也很自然：先看数据 → 再让 Google 看见你 → 最后才轮到挂广告赚钱。下面就按这个顺序来。

------

## 数据怎么看：给博客接上 Google Analytics 4

不知道读者从哪来、看了哪些文章、停留多久，就没法判断接下来该写什么方向、哪篇该重点优化。这一步几乎零成本，先装上，数据攒着，以后回头看才有意义。

### 申请 GA4 账号

打开 [analytics.google.com](https://analytics.google.com)，登录后点「开始衡量」（或左下角「管理」→「创建账号」），按引导填账号名称、资源（Property）名称，数据流类型选「网站」，填入你的博客域名。创建完成后，在「数据流详情」页面能看到一个 `G-` 开头的测量 ID，类似 `G-XXXXXXXXXX`，先复制下来。

### 接入 Stack 主题

Stack 主题原生支持 GA4，不用手动改任何模板文件——主题刚装上时 `config/_default/hugo.toml` 里是没有这部分配置的，需要自己手动加上去才会生效。打开这个文件，独立加上这一段（不用塞进已有的任何区块里）：

```toml
[services]
  [services.googleAnalytics]
    id = 'G-XXXXXXXXXX'
```

把 `G-XXXXXXXXXX` 换成你自己的测量 ID，保存。

> 网上不少老教程教的是 Universal Analytics（UA）的接入方式，那个版本已经停止收集数据了，认准 `G-` 开头的 GA4 测量 ID 就对了。

> 顺手检查一下 `params.toml` 里 `[cookies]` 的 `enabled` 字段（Stack 自带的 Cookie 同意横幅功能），这一个开关直接决定 GA 什么时候开始收集数据，两种结果正好相反：
>
> | `cookies.enabled` | 行为 |
> |---|---|
> | `true` | 页面会出现同意横幅，访客必须主动点"全部同意"或者在设置里勾选"分析"类别，GA 才开始收集数据；访客不操作 = 不收集 |
> | `false` | 不会出现任何横幅，GA 对所有访客直接加载、立即收集，不需要任何同意动作 |
>
> 容易搞反的是 `false` 这一项——它不是"关掉 cookies、所以不收集"，而是"不启用同意机制，直接收集"。如果接上 GA 之后实时报告迟迟没数据，先确认这个字段的值，再判断是不是访客还没点同意，还是配置本身没生效。

### 推送验证

```
git add .
git commit -m "接入 Google Analytics 4"
git push
```

等 Cloudflare 重新构建完（1-3分钟），打开博客随便点几个页面，回到 GA4 后台左侧「报告」→「实时」，看到自己的访问记录跳出来，就说明接通了。

### 备选方案：Cloudflare Web Analytics

如果你觉得 GA4 这套有点重，只是想看个访问量、来源国家这些基础数据，Cloudflare Pages 自带一个免费的 Web Analytics，不需要改任何配置文件：

**开启：** 登录 Cloudflare 后台，进 **Workers & Pages**，选中你的 Pages 项目（就是[上一篇部署教程](https://smallstep.one/hugo-cloudflare-deploy/)里创建的那个，不是另开新项目），点进 **Metrics** 标签，找到 **Web Analytics** 区域点 **Enable**。下次构建会自动把统计代码注入页面。

**看数据：** 不是在 Pages 项目里面看，是另一个独立入口——左侧菜单 **Observe** 分类下展开 **Analytics**，点 **Web analytics**，列表里找到你博客的域名点进去。

> Cloudflare 后台改版比较频繁，如果哪天菜单位置变了，认准「Observe → Analytics → Web analytics」这个层级关系去找就行。

它比 GA4 轻量，也不用处理 Cookie 同意横幅那一套隐私合规麻烦事，缺点是数据维度比 GA4 少很多，没有转化追踪、受众画像这些进阶功能。两个工具完全可以同时开着互相对照。

------

## 让 Google 知道你的博客存在：接入 Search Console

哪怕博客内容写得再好，Google 也不会自动知道这个网站存在——尤其是新域名，等 Google 自然爬到你的站可能要等很久。Search Console 的作用就是主动告诉 Google"这是我的网站，来抓取吧"，顺便还能看到读者用什么关键词搜到你、哪些页面在搜索结果里表现好。

### 验证网站所有权：先看你域名 DNS 在哪

打开 [search.google.com/search-console](https://search.google.com/search-console)，添加资源，这里分两种情况：

**情况一：域名 DNS 已经托管在 Cloudflare**（按照[上一篇部署教程](https://smallstep.one/hugo-cloudflare-deploy/)操作过的，大概率是这种情况）
选「网域」属性，输入你的域名，Google 会给一条 TXT 记录，去 Cloudflare 的 DNS 管理页面加上这条记录，回来点验证，通常几分钟内就能通过。

**情况二：域名 DNS 还在原平台，没有转入 Cloudflare**
选「网址前缀」属性，在输入框里填完整网址（带 `https://`，比如 `https://yourblog.com/`），点**继续**——这一步之后才会跳到验证方式页面，上面会有一排选项：HTML 文件、HTML 标记、Google Analytics、Google Tag Manager、域名名称提供商。挑「HTML 文件」最省事：点开它，下载 Google 给的验证文件（类似 `google1234567890abcdef.html`），放进 `myblog/static/` 目录，推送到 GitHub。Hugo 构建时会把 `static/` 目录里的文件原样复制到网站根目录，等 Cloudflare 重新构建完，这个文件就能在 `https://yourblog.com/google1234567890abcdef.html` 访问到，回 GSC 点验证就行，不需要改任何主题文件。

> HTML 标签验证法也能用，但需要把验证代码塞进 [自定义 head 文件](https://smallstep.one/hugo-stack-config/)（`layouts/_partials/head/custom.html`），步骤比直接丢文件到 `static/` 多一道，新手不如选 HTML 文件法省心。

### 提交 Sitemap（可选，但强烈建议做）

这一步不是验证完之后必须做的强制步骤——没有它，验证照样算通过，GSC 其他功能也照样能用。但对一个刚上线、几乎没有外部反向链接的新博客来说，主动提交能明显加快 Google 发现新页面的速度，比单纯等爬虫自己摸过来快很多，所以建议直接做掉。

Hugo 构建时会自动在网站根目录生成 `sitemap.xml`，不需要额外配置。具体操作还是在 [search.google.com/search-console](https://search.google.com/search-console) 这同一个后台里：

1. 验证通过后会自动跳进你这个网站对应的资源仪表盘（左上角能看到你的域名）
2. 左侧菜单找 **「索引」** 这个分类，展开后能看到「网页」「Sitemap」这几个子项——验证没通过之前这些功能区是展开不出来的
3. 点 **Sitemap**，右上角的输入框里填 `sitemap.xml`（不用填完整网址，只填这个文件名就行），点提交

提交后通常等一两天，「已发现的网页」数量才会开始增长，别提交完马上去刷新看结果。

------

## 把流量变成收入：申请 AdSense 前要知道的事

这一步很多人会在内容还没攒够的时候就急着申请，结果反复被拒还摸不清原因。这里先把现在的几个现实情况说清楚，具体的注册流程和广告位设置留到后面单独展开。

**门槛比想象中具体：** AdSense 官方明确要求网站要有"足够多的优质内容"，文字以完整的句子和段落呈现，不能是图片堆砌或者只有标题的"骨架站"。几篇文章就去申请，大概率会收到"网站内容太少"或"网站正在建设中"的拒绝理由。

**一稿多发是个隐藏坑：** 如果文章先发在知乎、公众号，再搬到博客上，Google 有可能认定博客上的内容不是"原创来源"，影响审核通过率。不少人反馈，把博客内容和其他平台同步发布的习惯改掉之后，申请才顺利通过。

**审核周期没有固定答案：** 官方给的预期是几天到 2-4 周，但社群里"提交十几次才过"的案例也不少见，每次被拒邮件通常不会给出具体原因，只会提示需要关注的方向。心态上提前留好这个不确定性，别指望提交了马上就能过。

**通过之后还有个小尾巴：** 如果用的是自定义域名，AdSense 账号关联后需要在网站根目录放一个 `ads.txt` 文件（Hugo 的话同样丢进 `static/` 目录就行），没放对会显示广告"未找到"的状态。

------

## 常见问题

**Q：GSC 显示"验证失败"**

A：「网址前缀 + HTML 文件」验证法最常见的问题是文件路径不对，确认验证文件确实放在 `myblog/static/` 目录的最外层（不是 `static/page/` 之类的子目录），并且已经推送、Cloudflare 已经构建完成再点验证。

**Q：GA4 后台一直没有数据**

A：先确认 `hugo.toml` 里的 `googleAnalytics` 字段确实保存并推送了；其次 GA4 的「实时」报告通常几秒内就有反应，但标准报告（用户数、会话数等）有 24-48 小时的延迟，别用标准报告来测试是否接通。

**Q：AdSense 显示广告"未找到"**

A：大概率是 `ads.txt` 没放对位置，或者放了但还没等 Cloudflare 重新构建生效，确认文件确实能在 `https://yourblog.com/ads.txt` 直接打开。

------

## 这篇之后，还有什么

这篇把数据、收录、变现三件事的起步打通了，但每一件都还有更细的玩法：广告位怎么摆放转化率更高、关键词怎么布局、收录率怎么进一步提升——这些留给中级05（SEO 进阶）专门展开。

------

## 系列导航

- 上一篇：中级03——主题视觉定制（即将发布）
- 下一篇：中级05——SEO 进阶（即将发布）
- [返回系列目录](https://smallstep.one/hugo-guide/)
