---
title: "7-Hugo + Cloudflare Pages 自动化部署完整教程（git push 即全球上线，2026 最新）"
date: 2026-03-28T19:54:30+08:00
draft: false
description: "手把手教你将 Hugo 博客托管至 Cloudflare Pages。借助 GitHub 实现自动化部署，享受免费且极致的边缘节点全球加速访问体验。"
keywords: ["Hugo 部署", "Cloudflare Pages", "自动化部署", "静态网站免费托管", "GitHub 持续集成 CI/CD", "博客零成本上线"]

url: "hugo-cloudflare-pages-deploy"
categories:
    - station
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

前六篇教程中，我们的网站仅运行在本地。今天是激动人心的一步：**让全世界都能访问我们的博客！**

在 2026 年，静态资源托管领域非常成熟。经过对比 Vercel、Netlify、GitHub Pages，本教程重点推荐 **Cloudflare Pages**，理由很简单：

- ✅ **免费额度极其慷慨**：每月 500 次构建，无限带宽，无限请求次数
- ✅ **全球 CDN 加速**：依托 Cloudflare 遍布全球的 300+ 节点，国内访问速度也很稳定
- ✅ **自动化程度高**：连接 GitHub 后，每次 `git push` 自动触发构建，无需任何手动操作
- ✅ **免费绑定自定义域名**：支持直接在后台一键绑定你的独立域名，并自动申请 SSL 证书

我们将实现这样的工作流：

```
本地写文章 → git push 提交到 GitHub → Cloudflare 自动拉取构建 → 全球瞬间上线
```

---

## 7.1 将本地项目推送到 GitHub

### 第一步：在 GitHub 新建仓库

登录 [github.com](https://github.com)，点击右上角 **"+"** → **New repository**，填写以下信息：

- **Repository name**：建议命名为 `my-hugo-blog` 或你喜欢的名字
- **可见性**：Public（公开）或 Private（私有）均可，Cloudflare Pages 两种都支持
- **不要勾选**任何初始化选项（README、.gitignore、License 都不选），保持仓库空白

点击 **Create repository**，GitHub 会跳转到一个空仓库页面，记住页面上显示的仓库地址（形如 `https://github.com/你的用户名/my-hugo-blog.git`）。

---

### ⚠️ 推送前的关键检查：处理主题的 .git 文件夹（最常见天坑）

在执行任何 git 命令之前，**必须先处理这个问题**，否则几乎 100% 会导致 Cloudflare 部署失败。

**问题原因：**

我们在第 3 篇安装 Stack 主题时，使用了 `git clone` 命令，这会在 `themes/hugo-theme-stack/` 目录下生成一个隐藏的 `.git` 文件夹。当你把整个博客项目推送到 GitHub 时，Git 会把这个带 `.git` 的主题文件夹识别为一个独立的"子模块（Submodule）"，而不是普通文件夹。

**导致的后果：**

Cloudflare Pages 在拉取你的源码时，会看到主题目录只是一个空的 Submodule 引用，**实际的主题文件根本不会被拉取过去**，构建时 Hugo 找不到主题，直接报错失败。

**解决方法（二选一）：**

**方法 A：直接删除主题的 .git 文件夹（推荐新手）**

找到 `themes/hugo-theme-stack/` 目录，删除其中的 `.git` 隐藏文件夹：

- Windows：在文件资源管理器中打开该目录，点击"查看"→勾选"隐藏的项目"，即可看到 `.git` 文件夹，直接删除。
- 或者在命令行执行：

```bash
# Windows (PowerShell)
Remove-Item -Recurse -Force themes/hugo-theme-stack/.git

# Mac/Linux
rm -rf themes/hugo-theme-stack/.git
```

删除后，主题目录就变成了普通文件夹，Git 会把它当作正常文件一起提交。

**方法 B：使用 .gitmodules 正式注册 Submodule（适合进阶用户）**

如果你希望保留 Submodule 的方式（方便以后用命令一键更新主题），需要在博客根目录执行：

```bash
git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/hugo-theme-stack
```

这会生成一个 `.gitmodules` 文件，Cloudflare Pages 能识别并正确拉取 Submodule。但后续更新主题的命令也会稍有不同（`git submodule update --remote`）。

> 💡 **小布建议：** 新手选方法 A，简单直接不出错。等你对 Git 熟悉了再考虑 Submodule。

---

### 第二步：初始化 Git 并推送

在博客根目录（如 `D:\web\smallstep.stack`）打开命令行，依次执行：

```bash
git init
git add .
git commit -m "Initial commit for Hugo blog"
git branch -M main
git remote add origin https://github.com/你的用户名/my-hugo-blog.git
git push -u origin main
```

推送过程中可能会弹出 GitHub 登录窗口，正常登录授权即可。

推送完成后，刷新 GitHub 仓库页面，应该能看到你的博客所有文件已经上传成功。

---

### 第三步（可选）：使用 GitHub Desktop 管理仓库

如果你不习惯命令行，**GitHub Desktop** 是更友好的选择。它提供可视化界面，让 commit、push、pull 变成几次点击的事。

下载地址：[desktop.github.com/download](https://desktop.github.com/download/)

安装后登录 GitHub 账号，点击 **Add an Existing Repository from your Hard Drive**，选择你的博客根目录，后续所有操作都可以在图形界面完成。

---

## 7.2 链接 Cloudflare Pages

### 第一步：进入 Workers & Pages

登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)，在左侧导航栏找到 **Compute（计算）** 分类下的 **Workers & Pages**，点击进入。

### 第二步：创建新项目

点击 **Create（创建应用程序）**，在页面下方找到"**想要部署 Pages？开始使用**"，点击 **导入现有 Git 存储库**。

### 第三步：授权并选择仓库

点击 **Connect to Git**，选择 **GitHub**，在弹出的授权窗口中登录并授权 Cloudflare 访问你的 GitHub 账号。

授权完成后，你会看到你的 GitHub 仓库列表，找到 `my-hugo-blog`，点击右侧的 **Select（选择）**。

### 第四步：配置构建参数（关键）

点击 **Begin setup（开始设置）**，进入构建配置页面。**以下每个参数都需要仔细核对：**

| 参数名 | 填写内容 | 说明 |
|--------|----------|------|
| Project name | 自定义名称（如 `smallstep-blog`） | 这将是你的默认三级域名：`名称.pages.dev` |
| Production branch | `main` | 对应你推送的分支名 |
| Framework preset | `Hugo` | 从下拉菜单选择，会自动填入构建命令 |
| Build command | `hugo --minify` | 加 `--minify` 启用资源压缩，性能更好 |
| Build output directory | `public` | Hugo 默认输出目录，保持不变 |

### 环境变量设置（不设置大概率报错）

在页面底部找到 **Environment variables（环境变量）**，展开后点击 **Add variable**，添加一条：

- **Variable name**：`HUGO_VERSION`
- **Value**：`0.158.0`

**为什么必须设置这个？**

Cloudflare 云端默认使用的 Hugo 版本较旧（有时是 0.54 这样的远古版本）。我们的项目用到了 Hugo v0.158 才支持的 `css.Build` 等新特性，如果云端版本太低，直接报错构建失败。指定版本号后，Cloudflare 会自动下载对应版本来构建，和你本地环境完全一致。

> 💡 **如何查看本地 Hugo 版本？** 在命令行运行 `hugo version`，把显示的版本号填入环境变量即可。

---

## 7.3 部署与验证

### 一键起飞

确认所有参数无误后，点击 **Save and Deploy（保存并部署）**。

Cloudflare 会立即开始构建，整个过程通常只需 **30秒到2分钟**。你可以在页面上实时看到构建日志。构建成功后，会显示一个绿色的 ✅ 标志，并给你分配一个访问链接：

```
https://your-project-name.pages.dev
```

打开这个链接，你的博客就正式上线了！

---

### 绑定自定义域名（可选但强烈推荐）

`.pages.dev` 的域名可以用，但有自己的独立域名会更专业，SEO 效果也更好。

在 Cloudflare Pages 项目页面，点击 **Custom domains（自定义域）** → **Set up a custom domain**，输入你的域名（如 `smallstep.one`）。

如果你的域名已经托管在 Cloudflare（即 DNS 由 Cloudflare 管理），几乎是一键完成，Cloudflare 会自动添加 CNAME 记录并签发 SSL 证书，整个过程 5 分钟内生效。

如果域名在其他平台（如 Namesilo、Spaceship），需要手动去域名商后台添加 CNAME 记录，Cloudflare 会告诉你具体填写什么内容。

---

### 验证自动化部署是否生效

绑定完域名后，做一个简单的测试：

1. 在本地随便打开一篇文章的 Markdown 文件，在末尾加一句话，比如"测试自动部署"
2. 保存后执行 `git add . && git commit -m "test auto deploy" && git push`
3. 打开 Cloudflare Dashboard，进入你的 Pages 项目，点击 **Deployments（部署）** 标签页
4. 你会看到一条新的部署记录正在进行，通常 1 分钟内完成
5. 刷新你的博客网站，确认修改已经上线

从此以后，写完文章 → `git push` → 全球上线，整个流程不需要你再做任何其他操作。

---

## 7.4 常见报错与解决方案

以下是部署过程中最高频出现的错误，每一个都有完整的解决步骤。

---

### ❌ 报错一：构建失败，提示找不到主题文件

**错误日志关键词：**
```
Error: failed to load modules: module "hugo-theme-stack" not found
```

**原因：** 主题目录的 `.git` 文件夹没有处理，导致 Cloudflare 拉取不到主题文件（参见 7.1 章节的天坑说明）。

**解决：** 回到本地，删除 `themes/hugo-theme-stack/.git` 文件夹，重新 commit 并 push。

---

### ❌ 报错二：构建失败，提示 Hugo 版本不兼容

**错误日志关键词：**
```
ERROR render of "page" failed: ... unknown function
```
或
```
Error: error building site: TOCSS: failed to transform "scss/custom.scss"
```

**原因：** 没有设置 `HUGO_VERSION` 环境变量，Cloudflare 用了旧版 Hugo 来构建。

**解决：** 在 Pages 项目 → Settings → Environment variables 里添加 `HUGO_VERSION = 0.158.0`，然后触发一次重新部署（点击 **Retry deployment**）。

---

### ❌ 报错三：push 时提示没有权限

**错误信息：**
```
remote: Permission to xxx/my-hugo-blog.git denied to xxx.
```

**原因：** GitHub 的认证 Token 过期，或者当前登录的账号没有仓库写入权限。

**解决：** 重新生成 GitHub Personal Access Token（GitHub → Settings → Developer settings → Personal access tokens → Generate new token），在命令行重新配置远程地址：

```bash
git remote set-url origin https://你的Token@github.com/你的用户名/my-hugo-blog.git
```

或者直接用 GitHub Desktop 操作，它会自动处理认证问题。

---

### ❌ 报错四：网站上线了但样式全部丢失，页面一片空白

**现象：** 网站可以访问，但没有任何样式，全是纯文字。

**原因：** `hugo.toml` 里的 `baseURL` 填写的是旧域名或 `localhost`，导致 CSS/JS 路径全部指向错误地址。

**解决：** 打开 `hugo.toml`，把 `baseURL` 改成你实际使用的域名：

```toml
baseURL = "https://smallstep.one/"  # 改成你的真实域名，结尾保留斜杠
```

修改后重新 push，Cloudflare 会自动重新构建。

---

### ❌ 报错五：每次 push 都触发构建，但有时候文章不更新

**现象：** Cloudflare 显示部署成功，但网站上看不到新文章。

**原因：** 文章的 Front Matter 里 `draft: true` 忘记改成 `false`，或者文章的 `date` 设置的是未来时间。

**解决：** 检查新文章的 Markdown 文件头部：

```yaml
---
title: "你的文章标题"
date: 2026-05-17        # 确认不是未来日期
draft: false             # 确认是 false 而不是 true
---
```

---

## 后续维护：日常写文章的完整流程

部署完成后，以后每次写文章的流程如下：

```
1. 在 content/post/ 目录下新建 Markdown 文件
2. 写完文章，确认 draft: false
3. 本地运行 hugo server 预览效果
4. 确认无误后：git add . → git commit -m "新增文章：xxx" → git push
5. 等待约 1 分钟，Cloudflare 自动部署完成，文章全球上线
```

---

## 系列导航

- **上一篇回顾：** [6-Hugo 美化进阶：Tailwind CSS + 暗黑模式 + 自定义样式](/hugo-tailwind-dark-mode/)
- **下一篇预告：** [8-Hugo 性能优化与 SEO 进阶：Hugo Pipes、sitemap、robots.txt 等](/hugo-performance-seo/)
- **返回 SmallStep 系列教程目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/categories/station/)