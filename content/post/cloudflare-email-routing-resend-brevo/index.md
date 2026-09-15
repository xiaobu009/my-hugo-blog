---
title: "用自定义域名免费收发邮件——Cloudflare + Resend/Brevo 完整配置指南"
date: 2026-07-19
lastmod: 2026-07-19
draft: false
description: "手把手教你用 Cloudflare Email Routing 接收域名邮件，配合 Resend 实现免费发信，全程在 Gmail 里统一管理，零成本拥有专业域名邮箱。"
keywords: ["Cloudflare", "域名邮箱", "Email Routing", "Resend", "Brevo", "免费邮件", "Gmail"]
categories:
  - 建站工具
tags:
  - Cloudflare
  - 域名邮箱
  - Resend
  - Brevo
  - Gmail
url: "cloudflare-email-routing-resend-brevo"
---

## 做完这篇你能得到什么

现今免费邮箱应该人人都有，但使用自己域名做后缀的邮箱，你是否也有？大家好，我是小布，今天我们就说说免费邮箱，以及怎么用自己的域名，创建可免费收发的邮件系统，配完之后：

- **收信**：任何人发邮件到 `contact@yourblog.com`，自动转发到你的 Gmail，秒到
- **发信**：在 Gmail 里直接用 `contact@yourblog.com` 的身份回复，对方看到的是你的域名
- **备用**：同时注册 Brevo 作为备用发信通道，Resend 出问题时五分钟内切换
- **全程免费**，不需要买任何付费服务

整个配置分三块：Cloudflare 收信、Resend 负发信、Gmail 作为统一的收发界面。跟着步骤做，你也能轻松搭建，拥有自己域名后缀的免费邮箱系统。

**前提条件：**

- 已经拥有自己的域名且已托管在 Cloudflare
- 一个 Gmail 账号用于接收转发邮件和发信

---

## 第一部分：收信配置——Cloudflare Email Routing

Cloudflare Email Routing 完全免费，配置完成后任何发往你域名的邮件都会自动转发到你的个人邮箱，不限转发地址数量。

### 进入 Email Routing

1. 登录 Cloudflare 仪表板（[dash.cloudflare.com](https://dash.cloudflare.com)），点击进入你的域名
2. 左侧菜单找到 **电子邮件（Email）→ Email Routing**

> **注意**：Cloudflare 已切换到新版 Email Routing 界面，没有旧版的引导向导。如果你进入后看到状态显示「已禁用 / 未配置」，不用慌，按下面的顺序手动配置即可。

### 第一步：添加并验证目标地址

目标地址就是你用来接收转发邮件的个人邮箱，必须先验证才能创建规则。

1. 点击 **目标地址** 标签页
2. 点击「添加目标地址」，填入你的 Gmail 地址
3. Cloudflare 会向该邮箱发一封验证邮件，去收件箱（没有就查垃圾箱）点击 **Verify email address**
4. 回到页面确认状态变为「已验证」

### 第二步：创建路由规则

1. 点击 **路由规则** 标签页
2. 点击「创建路由规则」，填写：
   - **自定义地址**：填你想要的前缀，比如 `contact`、`hi`、`support`
   - **操作**：选「发送到电子邮件」
   - **目标**：选刚才验证好的 Gmail 地址
3. 保存

### 第三步：开启 Catch-all（推荐）

Catch-all 是 Email Routing 最实用的功能之一。开启后，任何发往你域名下不存在地址的邮件都会自动转发到你的邮箱——不管前缀是什么。

1. 在「路由规则」页面找到 **Catch-all 地址**（中文界面显示为「全收」）
2. 点击右侧 **···** → 编辑
3. 注意：默认操作是「丢弃」，必须改为「发送到电子邮件」，目标选你的 Gmail
4. 保存后把右侧开关拨到「活跃」

> 开启 Catch-all 后，注册各类服务时可以随意编前缀，比如 `notion@yourblog.com`、`github@yourblog.com`，哪天收到垃圾邮件一眼就知道是谁泄露的。

### 第四步：启用路由并配置 DNS

1. 回到概览页，点击顶部「DNS 记录 — 未配置」旁边的箭头（或进入**设置**标签页）
2. 点击「添加记录并启用」，Cloudflare 会自动写入所需的 MX 和 TXT（SPF）记录，无需手动操作
3. 等几分钟，概览页状态变为「已启用」、DNS 变为「已配置」即完成

> **遇到 MX 记录冲突？** 说明这个域名之前配置过其他邮件服务，有旧的 MX 记录。按 Cloudflare 提示删除旧记录再启用，一个域名的收件只能指向一家服务。

### 验证收信

用另一个邮箱给 `contact@yourblog.com` 发一封测试邮件，几秒到一分钟内应该会出现在你的 Gmail 里。在「活动日志」标签页可以看到每封邮件的处理记录。

---

## 第二部分：发信方案——为什么选 Resend + Brevo

收信配好了，发信是另一回事。想用自己域名邮箱后缀发信，大概有以下几条路可以走：

1. **Cloudflare 自带的发信功能**——看起来最省事，但免费版完全不可用
2. **Outlook/Hotmail 账号做 SMTP**——额度够，但 2026 年已经基本堵死
3. **第三方发信服务（Resend / Brevo）**——免费、稳定、和 Gmail 完全兼容，推荐

下面逐一说明。

### 方案一：Cloudflare 自带发信——免费版不可用

Cloudflare 自己有发信功能，但看一眼计划对比就明白了：

| 指标 | 免费 | 付费（$5/月） |
|------|------|--------------|
| Workers 请求 | 100,000 / 天 | $0.30 / 百万请求 |
| CPU 时间 | 10 ms / 请求 | $0.02 / 百万 CPU ms |
| Workers 数量 | 100 | 500 |
| **邮件发送** | **—** | **包含** |

「—」代表免费版完全没有发信能力，必须升级到 $5/月 才能用。即使付费，官方文档也说明只能发送到账户内已验证的目标地址，无法随意对外发信。**对个人博客场景来说，花钱也买不到想要的效果，直接排除。**

### 方案二：Outlook/Hotmail 做 SMTP——这条路已经堵死

很多人会想到直接用 Outlook 账号做 SMTP，额度约 300 封/天，配置看起来也简单。但微软在 2026 年彻底废弃了「基础身份验证（Basic Auth）」：

- **2026 年 4 月 30 日起**：所有基础身份验证的 SMTP 连接全部被拒绝，无例外

Gmail 的「以其他地址发送」只支持传统账号密码方式，而 Outlook 现在强制要求 OAuth2 现代身份验证，两者根本无法配合。实际上从 2024 年 9 月底开始，通过 Gmail 用 Hotmail 发信就已经大范围报错。**这条路现在基本堵死了，不值得花时间折腾。**

### 方案三：Resend + Brevo——推荐方案

两个都是专门做发信的第三方服务，免费版完全够用，和 Gmail SMTP 无缝兼容：

| 对比项 | Resend | Brevo |
|--------|--------|-------|
| 免费额度 | 100 封/天，3000 封/月 | 300 封/天，9000 封/月 |
| 域名数量（免费版） | 最多 3 个 | 无明确限制 |
| Gmail SMTP 兼容 | ✅ | ✅ |
| 配置难度 | 低 | 低 |
| 定位 | 主力 | 备用，额度更高 |

Gmail 不允许同一个地址添加两条发件身份，所以无法同时把 Resend 和 Brevo 都接进来。实际方案是：**Resend 作为主力配进 Gmail，Brevo 注册好并验证域名备用**——万一 Resend 出问题，去 Gmail 删掉那条再用 Brevo 的 SMTP 参数重新添加，五分钟搞定。

或者直接设置两个不同的邮箱，比如contact1@smallstep.one，contact2@smallstep.one 分别对应 Resend及Brevo。

---

接下来进入实际配置阶段，一共四步：

1. **配置 Resend**：注册账号、在 Cloudflare 添加 DNS 记录、获取 SMTP 信息
2. **配置 Brevo**：注册账号、验证域名，备用
3. **在 Gmail 添加发件身份**：把 Resend 接进来，配置回复行为
4. **测试**：发一封邮件确认收发都正常

每步做完再进下一步，不要跳。DNS 记录生效需要几分钟，遇到验证没通过先等一等再试。

---

## 第三部分：配置 Resend

### 注册并添加域名

1. 去 [resend.com](https://resend.com) 注册账号
2. 进入左侧 **Domains** → 点击 **Add Domain**
3. **Name**：填入你的域名，比如 `yourblog.com`，不要加 `http://` 或 `/`
4. **Region**：选 **Tokyo（ap-northeast-1）**，离中文用户最近，选新加坡也行，如果有的话。
5. 点击 **Add Domain**，进入下一步

### 选择 Manual Setup

Resend 会询问如何配置 DNS，出现两个选项：

- **Auto configure**：授权 Resend 直接修改你的 Cloudflare DNS，权限过大
- **Manual setup**：Resend 列出记录，你自己去 Cloudflare 手动添加

选 **Manual setup**，更安全，也更清楚自己改了什么。

### 在 Cloudflare 添加 DNS 记录

Resend 会给你 4 条记录，逐一在 Cloudflare **DNS → 记录** 里添加：

| 类型 | Name | Content | 说明 |
|------|------|---------|------|
| TXT | `resend._domainkey` | `p=MIGfMA...`（点 Copy 复制完整内容） | DKIM 签名 |
| MX | `send` | `feedback-smtp.us-east-1.amazonses.com`（点 Copy 复制） | SPF 配套 |
| TXT | `send` | `v=spf1 include:...`（点 Copy 复制完整内容） | SPF 验证 |
| TXT | `_dmarc` | `v=DMARC1; p=none;` | DMARC（建议加） |

**添加时注意：**
- 在 Cloudflare 添加记录时，类型从下拉菜单选，不要手打
- **所有记录的代理状态必须关闭**（点橙色云图标变成灰色），DNS 验证类记录不能走 Cloudflare 代理
- Content 内容从 Resend 页面点 Copy 按钮复制，不要手打，避免出错
- `_dmarc` 如果之前没加过直接添加；如果已有则跳过，后面配 Brevo 时会用 Brevo 提供的更完整版本覆盖

全部添加完成后，回到 Resend 点击 **I've already added the records**，等几分钟，状态变为 **Verified** 即完成。页面上会显示「Domain verified: Your domain is ready to send emails.」

### 创建 API Key

1. 左侧菜单 **API Keys** → **Create API Key**
2. **Name**：随意填，比如 `gmail-smtp`
3. **Permission**：选 **Sending Access**（比 Full Access 更安全）
4. **Domain**：选 **All domains**（以后加新域名不需要重新建 Key）
5. 点击创建，**立刻复制保存这个 Key**，它只显示一次，关掉弹窗就看不到了

记下以下 SMTP 信息（所有 Resend 用户固定不变）：

```
Host：smtp.resend.com
Port：465
用户名：resend（固定，不是你的邮箱）
密码：你刚才复制的 API Key（re_ 开头）
加密：SSL
```

---

## 第四部分：配置 Brevo（备用）

Brevo 免费版 300 封/天，作为备用通道注册好、域名验证完，平时不需要动。

### 注册账号

1. 去 [brevo.com](https://brevo.com) 注册账号
2. 注册过程中会询问团队规模、联系人数量等，随意填写，不影响功能
3. **Do you sell online?**：选 **no**
4. 建议勾选「I don't want to receive product updates...」拒绝营销邮件
5. 填写公司地址信息，可以随意填，不会被核实
6. **手机验证**：需要填真实手机号接收验证码，点击国旗切换到 +86，填入手机号

### 获取 SMTP 信息

注册完成进入 Brevo 后，左侧菜单点 **Transactional**，会自动跳到 SMTP 配置页面，记下以下信息：

```
Host：smtp-relay.brevo.com
Port：587
用户名：页面上 Login 那行显示的地址（@smtp-brevo.com 结尾，不是你的登录邮箱）
密码：页面上 Password 那行显示的内容
加密：TLS
```

> **注意**：Brevo 的用户名是系统分配的专属地址（格式类似 `b28032001@smtp-brevo.com`），不是你注册时用的邮箱，配 Gmail 时填这个。

### 添加并验证域名

直接在浏览器访问以下地址跳转到域名管理页：

```
https://app.brevo.com/senders/domain/list
```

1. 点击 **Add a domain**，填入你的域名
2. **Branded subdomain**：填 `mail`（Brevo 推荐值，让追踪链接显示你自己的域名）
3. **Setup method**：选 **Manual**（同 Resend，安全起见手动添加）
4. 下一步 Brevo 会给你 6 条 DNS 记录

### 在 Cloudflare 添加 DNS 记录

| 类型 | Name | Content | 必须/可选 |
|------|------|---------|----------|
| CNAME | `mail` | `mail-yourblog-com.brand.brevosend.com` | 必须 |
| TXT | `@` | `brevo-code:...`（点 Copy 复制完整内容） | 必须 |
| CNAME | `brevo1._domainkey` | `b1.yourblog-com.dkim.brevo.com` | 必须 |
| CNAME | `brevo2._domainkey` | `b2.yourblog-com.dkim.brevo.com` | 必须 |
| TXT | `_dmarc` | `v=DMARC1; p=none; rua=mailto:...`（点 Copy 复制完整内容） | 建议加 |
| CNAME | `img.mail` | `mail-yourblog-com.img.brand.brevosend.com` | 可选 |
| CNAME | `r.mail` | `mail-yourblog-com.r.brand.brevosend.com` | 可选 |

**几个注意事项：**
- `@` 这条 TXT 记录：Name 填 `@`，在 Cloudflare 里代表根域名
- `_dmarc` 这条：如果配 Resend 时已经加过，用 Brevo 提供的更完整版本直接覆盖原有内容即可
- 所有记录代理状态关闭（灰色云）
- 后两条（img.mail、r.mail）是邮件图片和链接品牌化记录，个人博客可以不加

全部添加完成后，回到 Brevo 点 **Verify records**，验证通过后点 **Authenticate domain**，看到「Domain authenticated and branded」弹窗说明完成。

> **Branding status 显示 Not branded？** 正常现象，不影响发信功能，忽略即可。

---

## 第五部分：在 Gmail 添加发件身份

### 添加 Resend 发件身份

1. 打开 Gmail → 右上角齿轮 → **查看所有设置**
2. 切到 **账号和导入** 标签页
3. 找到「用这个地址发送邮件」→ 点击**添加其他电子邮件地址**
4. 填写发件人信息：
   - **名称**：你想让对方看到的名字，比如 `小布` 或 `xiaobu`
   - **邮箱**：`contact@yourblog.com`
   - **取消勾选**「视为别名」
5. 点击「下一步」

> **Gmail 会自动检测域名 MX 记录并预填 Cloudflare 的 SMTP 信息，这些都需要手动改掉。**

6. 填写 Resend 的 SMTP 信息：
   - **SMTP 服务器**：`smtp.resend.com`
   - **端口**：`465`
   - **用户名**：`resend`
   - **密码**：你的 Resend API Key
   - **加密**：选「使用 SSL 的安全连接」
7. 点击「添加账号」

### 验证发件身份

Gmail 会向 `contact@yourblog.com` 发一封验证邮件。因为你已经配好了 Cloudflare Email Routing，这封邮件会自动转发到你的 Gmail 收件箱。找到后点击确认链接，发件身份即生效，状态从「未确认」变为正常。

### 配置回复行为

默认情况下，回复邮件时 Gmail 会用默认地址（你的 Gmail）而不是收件地址回复。需要修改这个设置：

在同一个「账号和导入」页面，找到「回复邮件时」选项：

- ○ 始终以默认地址回复
- ● **用此相同地址回复**（选这个）

选好后页面会自动保存。**注意：已打开的回复窗口不会立即刷新，需要关闭后重新点「回复」才能生效。**

---

## 第六部分：测试收发

### 测试收信

用另一个邮箱发一封测试邮件到 `contact@yourblog.com`，确认几秒到一分钟内出现在 Gmail 收件箱里。

### 测试发信

在 Gmail 里回复那封测试邮件，检查发件人是否自动切换为 `contact@yourblog.com`。如果没有自动切换，点击发件人旁边的下拉箭头手动选择域名邮箱地址。

对方收到邮件后，发件人显示的应该是 `xiaobu <contact@yourblog.com>`，而不是你的 Gmail 地址。

---

## 第七部分：多域名扩展

如果你有多个域名，两个平台都支持：

- **Resend**：免费版最多添加 **3 个域名**，每个单独验证 DNS
- **Brevo**：免费版**无明确域名数量限制**，可以添加多个

扩展方式：在 Resend/Brevo 里重复「添加域名 → Cloudflare 加 DNS 记录 → 验证」的流程，然后在 Gmail 的「以其他地址发送」里再加一条新域名的地址即可。所有域名的邮件都在同一个 Gmail 界面里统一收发。

---

## 常见问题

**Q：Catch-all 开启后，垃圾邮件会不会很多？**

A：Cloudflare Email Routing 自带垃圾邮件过滤和钓鱼检测，Gmail 本身也有强力过滤，实际骚扰邮件并不多。如果某个前缀收到太多垃圾邮件，可以在路由规则里单独为那个地址创建一条「丢弃」规则，优先级高于 Catch-all。

**Q：Gmail 添加发件身份时提示「无法连接到 SMTP 服务器」**

A：检查端口和加密方式是否填对——Resend 用 465 + SSL，Brevo 用 587 + TLS，两者不一样，别填混了。另外确认 API Key 或 Brevo 密码没有复制多余的空格。

**Q：Resend 出问题了怎么切换到 Brevo？**

A：去 Gmail「账号和导入」设置，删掉 `contact@yourblog.com` 那条，重新点「添加其他电子邮件地址」，地址还是填 `contact@yourblog.com`，但 SMTP 信息改填 Brevo 的参数（smtp-relay.brevo.com，587，TLS，你的 Brevo Login 和 Password）。

**Q：验证邮件发出去很久没收到**

A：先查 Gmail 垃圾箱。如果还没有，去 Cloudflare Email Routing 的「活动日志」标签页看这封邮件有没有被接收，判断是转发环节还是 DNS 没生效的问题。

**Q：对方回复我时，看到的是 Gmail 还是域名邮箱？**

A：用 `contact@yourblog.com` 发出去，对方回复时收件人就是 `contact@yourblog.com`，经 Email Routing 转发后还是进你的 Gmail，整个链路都是域名邮箱，对方看不到你的 Gmail 地址。

**Q：Resend 和 Brevo 可以用同一个域名吗？**

A：可以，两个平台各自验证一次 DNS（各自添加不同的 DKIM 记录），互不影响。

---

## 总结

配完这篇，你拥有了：

- 一个基于 Cloudflare 的域名收信系统，任意前缀都能收，Catch-all 兜底不漏邮件
- Resend 主力发信通道，100 封/天免费额度日常完全够用
- Brevo 备用账号已就绪，需要时五分钟内切换
- Gmail 作为统一界面，收发全在一个地方，不需要开新邮箱或切换 App

整套方案零成本，配好之后基本不需要维护，`contact@yourblog.com` 从此就是你的正式对外联系邮箱了。

---

## 系列导航

本文为独立教程，不属于 Hugo 建站系列。如果你的博客还在搭建阶段，可以从这里开始：

- [零基础搭建个人博客——Hugo + Stack 4.0 本地环境完整指南](https://smallstep.one/hugo-local-setup/)
- [免费把博客发布到全球——GitHub + Cloudflare Pages 部署完整指南](https://smallstep.one/hugo-cloudflare-deploy/)
- [返回博客首页](https://smallstep.one/)
