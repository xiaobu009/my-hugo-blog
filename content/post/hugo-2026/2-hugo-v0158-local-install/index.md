---
title: "2-Hugo v0.158 本地安装 + 创建第一个站点（Windows 完整图文教程）"
date: 2026-03-26T11:37:23+08:00
lastmod: 2026-05-17
draft: false
description: "手把手教你在 Windows 上安装 Hugo v0.158 Extended 版本，配置环境变量，创建第一个站点并在本地预览。包含 Git 和 VS Code 安装说明，以及新手最常遇到的 5 个报错解决方案。"
keywords: ["Hugo 安装 Windows", "Hugo 环境变量配置", "Hugo extended", "hugo new site", "Hugo 本地预览", "hugo server", "Git 安装", "VS Code Hugo"]
url: "hugo-v0158-local-install"
categories:
    - station
tags:
    - Hugo
    - 静态站点
    - 建站教程
---

上一篇我们聊清楚了为什么选 Hugo。这一篇正式开始动手——**在本地把 Hugo 跑起来，创建你的第一个站点，并在浏览器里看到它**。

整个过程不需要任何编程基础，跟着一步步做就行。预计耗时 **20-30 分钟**。


## 准备工作：需要安装哪些工具？

在安装 Hugo 之前，有两个工具需要先装好：

| 工具 | 用途 | 是否必须 |
|------|------|----------|
| **Git** | 版本控制，后续推送到 GitHub 必需 | ✅ 必须 |
| **VS Code** | 编辑 Markdown 文章和配置文件 | 推荐，用其他编辑器也可以 |
| **Hugo Extended** | 主角，静态网站生成器 | ✅ 必须 |

---

## 2.1 安装 Git

Git 是我们后续把博客推送到 GitHub、触发 Cloudflare 自动部署的基础工具，必须先装。

**下载地址：** [git-scm.com/download/win](https://git-scm.com/download/win)

打开页面后点击 **64-bit Git for Windows Setup** 下载安装包，文件约 65MB。

**安装过程中有几个选项需要注意：**

**① 选择默认编辑器**

安装向导会问你"Git 的默认编辑器用哪个"，默认是 Vim（对新手不友好）。建议从下拉菜单里改选 **Use Visual Studio Code as Git's default editor**，这样后面遇到 Git 需要输入提交信息时，会自动打开 VS Code，更顺手。

**② 调整 PATH 环境（重要）**

这一步选择 **Git from the command line and also from 3rd-party software**（推荐选项，默认就是这个），确保 Git 命令能在 Windows 命令行里直接使用。

**③ 其余选项全部保持默认**，一路 Next 直到安装完成。

**验证安装成功：**

按 `Win + R`，输入 `cmd`，回车打开命令提示符，输入：

```bash
git --version
```

如果显示类似 `git version 2.47.0.windows.1` 就说明安装成功了。

---

## 2.2 安装 VS Code（可选但推荐）

VS Code 是目前最流行的代码编辑器，完全免费，写 Markdown 文章和编辑配置文件都非常顺手。

**下载地址：** [code.visualstudio.com](https://code.visualstudio.com/)

点击蓝色的 **Download for Windows** 按钮下载，安装过程全部默认即可。

**安装后推荐装两个插件（在 VS Code 里按 `Ctrl+Shift+X` 打开插件面板搜索）：**

- **Markdown All in One**：Markdown 语法高亮、快捷键、实时预览
- **TOML Language Support**：让 `hugo.toml` 配置文件有语法高亮，不容易写错

---

## 2.3 安装 Hugo Extended

这是最关键的一步。注意：**一定要安装 Extended（扩展版）**，不要装普通版。

Stack 主题和很多其他主题使用了 SCSS 样式文件，只有 Extended 版本才支持编译 SCSS。普通版安装后运行会直接报错。

### 下载正确的版本

打开 Hugo 的 GitHub Releases 页面：[github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)

找到最新版本（本教程使用 v0.158.0），在 Assets 列表里找到这个文件：

```
hugo_extended_0.158.0_windows-amd64.zip
```

**关键词：** 文件名必须包含 `extended`，系统选 `windows-amd64`。

> 💡 **网络访问问题：** 如果 GitHub 下载缓慢，可以用 GitHub 镜像加速服务，在 URL 前加 `https://mirror.ghproxy.com/` 即可。

下载后解压，你会看到三个文件：

```
hugo.exe        ← 主程序，这是我们需要的
LICENSE
README.md
```

### 配置环境变量（让系统认识 hugo 命令）

**第一步：** 在 C 盘根目录创建一个文件夹，命名为 `hugo`，然后在里面再创建一个 `bin` 文件夹。完整路径是：

```
C:\hugo\bin\
```

**第二步：** 把刚才解压出来的 `hugo.exe` 复制到 `C:\hugo\bin\` 里。

**第三步：** 配置系统环境变量：

1. 右键「此电脑」→「属性」
2. 点击「高级系统设置」→「环境变量」
3. 在上方「用户变量」区域找到 **Path**，双击打开
4. 点击「新建」，输入：`C:\hugo\bin`
5. 连续点击「确定」保存

**第四步：** 验证是否成功。**重新打开**一个新的命令提示符窗口（已经打开的不会刷新环境变量），输入：

```bash
hugo version
```

如果显示类似下面这行，说明安装成功：

```
hugo v0.158.0-xxxxxxxx+extended windows/amd64 BuildDate=...
```

注意确认版本号里有 `+extended` 字样，这证明你安装的是扩展版。

---

## 2.4 创建你的第一个 Hugo 站点

工具都装好了，现在来创建站点。

### 选择站点存放位置

建议把博客项目放在一个路径简单、没有中文和空格的目录下，避免各种奇怪的路径问题。

例如：`D:\web\my-blog\`

### 执行建站命令

打开命令提示符，切换到你想存放博客的目录，执行：

```bash
# 进入 D 盘的 web 文件夹（如果没有这个文件夹先手动创建）
cd D:\web

# 创建一个名为 my-blog 的 Hugo 站点
hugo new site my-blog
```

看到这样的输出，说明站点创建成功：

```
Congratulations! Your new Hugo site was created in D:\web\my-blog.

Just a few more steps...
1. Change the current directory to D:\web\my-blog.
2. Create or install a theme:
   - Create a new theme with the command "hugo new theme <THEMENAME>"
   - Install a theme from https://themes.gohugo.io/
...
```

### 了解站点的目录结构

用 VS Code 打开 `D:\web\my-blog` 文件夹，看一下 Hugo 自动生成的目录结构：

```
my-blog/
├── archetypes/          # 文章模板，新建文章时自动套用
│   └── default.md
├── assets/              # 需要 Hugo 处理的资源（SCSS、JS 等）
├── content/             # 你的所有文章都放在这里
├── data/                # 数据文件（JSON、YAML、TOML）
├── i18n/                # 多语言翻译文件
├── layouts/             # 自定义模板（覆盖主题默认模板用）
├── static/              # 静态资源（图片、favicon 等）
├── themes/              # 主题文件夹
└── hugo.toml            # 站点核心配置文件 ← 最重要
```

**现在最重要的三个目录：**

- `content/`：你所有的文章 Markdown 文件都放这里
- `themes/`：主题文件装这里（下一篇教程会安装 Stack 主题）
- `hugo.toml`：站点标题、域名、主题设置等核心配置都在这里

---

## 2.5 修改基础配置

用 VS Code 打开 `hugo.toml`，你会看到：

```toml
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
```

先做几处基础修改：

```toml
baseURL = 'http://localhost:1313/'   # 本地开发阶段先用这个，上线后改成真实域名
languageCode = 'zh-cn'              # 改成中文
title = '你的博客名称'               # 改成你自己的博客标题
hasCJKLanguage = true               # 加上这行，让中文字数统计和阅读时长计算正确
```

> 💡 **`hasCJKLanguage = true` 是什么？** 告诉 Hugo 这个站点包含中日韩文字，启用正确的字符计数逻辑。不加这行，中文文章的"阅读时长"会显示成"1分钟"，因为 Hugo 把每个汉字都当成一个英文字母来统计字数。

---

## 2.6 创建第一篇文章

在命令行进入站点目录，执行：

```bash
cd D:\web\my-blog
hugo new content post/my-first-post/index.md
```

Hugo 会在 `content/post/my-first-post/` 下自动创建一个 `index.md` 文件，并套用 archetypes 里的模板，内容大概是：

```yaml
---
title: "My First Post"
date: 2026-05-17T10:00:00+08:00
draft: true
---
```

用 VS Code 打开这个文件，在 `---` 下方写点内容：

```markdown
---
title: "我的第一篇文章"
date: 2026-05-17T10:00:00+08:00
draft: false
---

你好，这是我用 Hugo 写的第一篇文章！

## 这是一个二级标题

这里写文章正文，Hugo 使用标准 Markdown 语法。
```

**注意：** 把 `draft: true` 改成 `draft: false`，否则文章不会显示（draft 表示草稿状态）。

---

## 2.7 在本地预览站点

在站点根目录执行：

```bash
hugo server -D
```

参数说明：
- `hugo server`：启动本地开发服务器
- `-D`：同时显示 `draft: true` 的草稿文章（方便写作时预览）

看到类似这样的输出，说明服务器启动成功：

```
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

打开浏览器，访问 `http://localhost:1313/`，你就能看到你的博客了。

目前没有安装主题，页面会非常简陋——这是正常的。**下一篇教程我们会安装 Stack 主题**，安装完之后整个博客就有了完整的样式和布局。

**本地预览的两个好用特性：**

1. **热更新**：保持 `hugo server` 运行状态，修改任何文章或配置文件后，浏览器会**自动刷新**，不需要手动重启服务器
2. **草稿可见**：加了 `-D` 参数，`draft: true` 的文章在本地也能看到，方便写作时预览效果

---

## 2.8 常见报错与解决方案

### ❌ 报错一：`hugo` 不是内部或外部命令

**错误信息：**
```
'hugo' 不是内部或外部命令，也不是可运行的程序或批处理文件。
```

**原因：** 环境变量配置没有生效，或者配置路径写错了。

**解决：**
1. 确认 `C:\hugo\bin\hugo.exe` 文件存在
2. 确认环境变量 Path 里加的是 `C:\hugo\bin`（不是 `C:\hugo\bin\hugo.exe`）
3. **关闭所有命令提示符窗口，重新打开一个新的**再试（环境变量修改后需要重启终端才能生效）

---

### ❌ 报错二：hugo version 显示没有 +extended

**显示内容：**
```
hugo v0.158.0-xxxxxxxx windows/amd64
```
（没有 `+extended`）

**原因：** 你下载的是普通版，不是扩展版。

**解决：** 回到 GitHub Releases 页面，下载文件名包含 `extended` 的版本，替换掉 `C:\hugo\bin\hugo.exe`。

---

### ❌ 报错三：hugo server 报错 TOCSS 或 SCSS 相关错误

**错误信息关键词：**
```
ERROR TOCSS: failed to transform "scss/..."
```

**原因：** 和报错二一样，安装的是普通版 Hugo，不支持 SCSS 编译。

**解决：** 同上，替换为 Extended 版本。

---

### ❌ 报错四：端口 1313 被占用

**错误信息：**
```
Error: listen tcp 127.0.0.1:1313: bind: Only one usage of each socket address
```

**原因：** 已经有一个 `hugo server` 进程在运行，占用了 1313 端口。

**解决：** 按 `Ctrl+C` 停止当前运行的服务器，或者用不同端口启动：

```bash
hugo server -D --port 1314
```

---

### ❌ 报错五：文章写完了但浏览器看不到

**现象：** `hugo server` 正常运行，但刷新浏览器看不到新写的文章。

**原因（两种可能）：**
1. 文章的 Front Matter 里 `draft: true` 没有改成 `draft: false`
2. 运行的是 `hugo server`（没有 `-D` 参数），草稿文章不显示

**解决：** 检查文章头部，确认 `draft: false`，或者启动服务器时加上 `-D` 参数。

---

## 本篇小结

到这里，你已经完成了：

- ✅ Git 安装和验证
- ✅ Hugo Extended 安装和环境变量配置
- ✅ 第一个 Hugo 站点创建
- ✅ 本地预览服务器启动

**下一步是什么？**

现在的博客页面很简陋，没有任何样式，因为还没有安装主题。下一篇我们来安装 Stack 主题，配置好之后你的博客就会有完整的三栏布局、暗色模式、文章分类、标签云等功能，颜值直接拉满。

---

## 系列导航

- **上一篇：** [1-2026 年为什么还选 Hugo？从 Notion + NotionNext 踩坑说起](/2026-hugo-why/)
- **下一篇：** [3-Stack 主题安装与基础配置](/2026-best-hugo-themes/)
- **返回系列目录：** [2026 年从零开始 Hugo 建站入门到进阶系列](/categories/station/)
