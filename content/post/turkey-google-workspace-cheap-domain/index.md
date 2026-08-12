---
title: "每年不到11元的.com域名——Google官方渠道购买，设置+托管到Cloudflare全教程"
date: 2026-08-04
lastmod: 2026-08-04
draft: false
description: "手把手实测：通过 Google Workspace ，用不到11元人民币注册一个.com域名，并把它正确托管到 Cloudflare，含 Squarespace 后台操作和 DNSSEC 设置全流程。"
keywords: ["Google Workspace", "土耳其区域名", "便宜域名", "薅羊毛", "Cloudflare", "Squarespace"]
categories:
  - Technology
tags:
  - Google Workspace
  - 域名
  - Cloudflare
  - Squarespace
url: "turkey-google-workspace-cheap-domain"
---

{{< youtube s6g7IKIJGSw  >}}

购买一个.com 域名，目前市面上多少钱？如果不到11元人民币，感觉够不够便宜，反正到目前为止，我还没碰到过更便宜的.com域名——别人一年七八十的成本，用这个方法能省去五六十，你说值不值。

这个方法走的是谷歌官方渠道，完全合规，这是一套完整实测流程，包含了从workspace账号注册，域名购买，托管到Cloudflare，到workspace服务订阅与取消的整个流程，以及提前取消workspace订阅，无法在Squarespace平台上，完成谷歌新账号登录验证的解决办法。只要跟着步骤操作，你也能拥有一个十块出头的 .com 域名。

------

## 主流域名平台 .com 价格对比

先把账算清楚，看这套流程值不值得折腾一次：

| 平台                | 首年价格（人民币） | 续费价格（人民币） | 备注                               |
| ------------------- | ------------------ | ------------------ | ---------------------------------- |
| GoDaddy             | 促销价可到几块钱   | 约150元+           | 首年低续费贵，消费者保护条款变化快 |
| Namecheap           | 较便宜             | 约120元            | 首年友好，续费涨得快               |
| Spaceship           | 约80元             | 约80元             | 首年续费同价，界面简洁             |
| NameSilo            | 约100元            | 约100元            | 首年续费同价，送WHOIS隐私保护      |
| Cloudflare          | 约75元             | 约75元             | 不加价，但要求用 Cloudflare 做 DNS |
| **谷歌 Workspace** | **约10-15元**      | **约10-15元**      | 汇率套利，土耳其区                 |

除了土耳其区这条路，其他平台基本都是"续费≈首年"或者"首年低续费贵"两种模式，没有谁能常年做到10块钱出头，这也是折腾一次的意义所在。

------

## 准备工作

- **一张能境外支付的信用卡**（Visa/Mastercard 均可，不要求土耳其发卡，账单地址不用改），虚拟卡容易触发风控，能用实体卡就别用虚拟卡
- **一个土耳其地址**，搜"土耳其地址生成器"能找到多个，生成一份格式正确的地址直接复制粘贴即可
- **一个用于接收验证邮件的邮箱**，用来注册时留联系方式，之后 Squarespace 会发验证邮件到这里
- 心理预期：整个流程会经过 Workspace 和 Squarespace 两套后台来回跳转，耐心走完就行

------

这个工具就是：**Google Workspace**，它是 Google 推出的一套 **企业办公套件**（含 Gmail、云盘、文档等）。它支持在workspace内购买域名，且支持使用不同地区溢价进行支付，今天我们就是通过Workspace，申请自己专属的 **.com** 域名。

## 第一步：在 Workspace 里购买域名

开始前先交代一句背景：Google 已经把域名注册商业务整体卖给了 **Squarespace**，所以走完这套流程，购买之后的域名管理、验证、改 DNS，全都是在 Squarespace 后台完成的，不再是纯 Google 界面。这跟网上大部分老教程的截图对不上，是最容易让人卡壳的地方，后面遇到跳转到 Squarespace 不用慌，是正常流程。

打开 [workspace.google.com](https://workspace.google.com/intl/zh-CN/)，点击「开始免费试用」。

1. **创建新账号**——不需要提前有 Gmail，这里会引导你新建一个专门用来管理这个域名的 Google 账号
![](/images/wp-009.webp)
2. **填写公司信息**：公司名称随便填，员工人数选"只有您一人"或类似选项，**区域一定要选土耳其**，这是价格便宜的关键
![](/images/wp-010.webp)
3. **填写联系信息**：姓名随意，联系邮箱可以用非 Gmail 邮箱（Outlook、QQ邮箱等均可）
![](/images/wp-011.webp)
4. **选择设置账号的方式**：选「获取新的自定义域名」，点继续
![](/images/wp-012.webp)
5. **搜索域名**：输入你想要的域名，能看到 .com/.info 等后缀显示的是里拉计价，通常是**75里拉/年**（约合10-15元人民币），已被注册的会直接提示不可用，可用的话直接进入下一步
![](/images/wp-013.webp)
6. **填写企业信息**：这里"将我的联系信息设为不公开"默认是开启的，也就是免费送 WHOIS 隐私保护。因为选的是土耳其地区，这里要填前面准备好的土耳其地址
![](/images/wp-014.webp)
7. **创建用户**：这个用户名就是`用户名@你的新域名`，比如 `admin@yourname.com`，用户名随意取，**这个账号密码要记好**，后面登录 Workspace 后台和 Squarespace 都要用它
![](/images/wp-015.webp)
8. **选择服务套餐**：会看到 Workspace 商务标准版的介绍，14天免费试用，试用期结束才开始扣费，试用期内可随时取消。**这里要注意：域名费用和 Workspace 订阅费用是分开计费的两笔钱**，点击「开始试用」
![](/images/wp-016.webp)
9. **确认订单信息**：会看到 Workspace 应用当前费用为0，域名费用是75里拉，页面会自动跳转到支付页
![](/images/wp-017.webp)
10. **绑定付款方式**：填入信用卡信息，确认结账
![](/images/wp-018.webp)
11. **是否添加新用户**：点「暂时跳过」即可
![](/images/wp-019.webp)
12. **个性化设置**：三项选项都可以选"我还不确定"，不影响后续使用
![](/images/wp-020.webp)

------

## 第二步：完成 Squarespace 邮箱验证（容易被漏掉的一步）

个性化设置走完之后，**[Squarespace](https://www.squarespace.com/)**（不是 Workspace）会发一封激活邮件到你留的联系邮箱——这是因为域名业务已经转给 Squarespace 管理了。  
![](/images/wp-021.webp)

1. 打开这封邮件，点击 **verify now** 按钮
2. 会提示用 **Continue with Google** 登录，这里登录的账号就是刚才新建的那个（`用户名@你的域名`）
![](/images/wp-022.webp)
![](/images/wp-023.webp)
3. 登录成功后会跳转到 Squarespace 后台的 Domains 页面，能看到刚买的域名。Squarespace 要求**15天内完成邮箱验证**，逾期可能影响域名状态
4. 回到验证邮件点击验证，看到 **Email Successfully Verified** 就说明验证成功，域名在 Squarespace 那边会显示为激活状态

到这一步，域名购买才算真正完成。

------

## 第三步：⚠️ 关键提醒——托管到 Cloudflare 之前，先别取消订阅

购买完成后，回到 Workspace 管理后台，订阅列表下会看到两条独立记录：**Workspace 商务标准版**和**域名注册**。Workspace 那笔会显示"付费服务将于X天后开始"，说明还在试用期。

**这里千万不要急着取消 Workspace 订阅**，哪怕你只想留着域名、不想用 Workspace 服务。原因下一节详细说，先记住这个顺序：**必须先把域名托管到 Cloudflare，之后再考虑要不要取消订阅**。

------

## 第四步：域名托管到 Cloudflare

### Cloudflare 那边的操作

1. 登录 [dash.cloudflare.com](https://dash.cloudflare.com)，点「添加域名」，输入刚买的域名，点继续
2. 选免费计划，Cloudflare 会自动扫描域名已有的 DNS 配置，点「继续前往激活」
3. Cloudflare 会分配两个专属的 Nameserver 地址，**保持这个页面打开不要关**，去 Squarespace 那边操作

### Squarespace 那边的操作

登录 Squarespace 管理后台，进入 **Domains** → **DNS** → **Domain Nameservers**，点击右上角 **USE CUSTOM NAMESERVERS**。

这一步会要求用 Google 账号登录验证——**用之前新建的那个账号**（`用户名@你的域名`）登录。

> **⚠️ 最关键的一个坑：** 如果在这一步之前就已经取消了 Workspace 订阅，这里的谷歌账号登录验证会直接被拦，提示 `accounts.google.com is blocked`，导致改不了 Nameserver。解决办法见下方「补救办法」这一节。

验证通过后，会看到 Nameserver 设置页面，把 Cloudflare 给的两个 Nameserver 地址分别粘贴进去保存。如果你的域名开了 DNSSEC，系统会提示"修改 Nameserver 会自动关闭 DNSSEC"，这是必要步骤，直接确认继续即可。

保存后会弹出 **Changes Submitted** 提示框，问你要不要 **Enable DNSSEC**，这里**直接关闭窗口就行**，DNSSEC 要等 Nameserver 真正切到 Cloudflare 之后再单独设置（见下一节）。

### 回到 Cloudflare 确认

回到 Cloudflare 页面，往下拉点击「我已更新名称服务器」。页面会提示预计需要1-2小时，但实际通常几分钟就能刷出结果，看到「您的域名现在受到 Cloudflare 保护」就说明托管成功了。

------

## 第五步：重新开启 DNSSEC（可选）

DNSSEC 是防止 DNS 记录被篡改的安全机制，开不开都不影响正常使用，看你自己需要。要开的话，正确顺序是拿 Cloudflare 生成的参数手动填到 Squarespace，不能直接点"一键开启"。

1. 在 Cloudflare 后台点进这个域名，找到 **DNS** → 设置项，点击「启用 DNSSEC」，会生成一份包含摘要、摘要类型、算法、密钥标记的 DS 记录
2. 回到 Squarespace 后台，进入 **DNS** → **DNSSEC**，点击 **ADD RECORD**，把 Cloudflare 给的四项参数原样填进去：

| Cloudflare 字段 | 对应填入 Squarespace |
|------|------|
| 密钥标记（Key Tag） | KEY TAG |
| 算法（Algorithm） | ALGORITHM |
| 摘要类型（Digest Type） | DIGEST TYPE |
| 摘要（Digest） | DIGEST |

3. 保存后等待生效即可

------

## 第六步：如何正确取消订阅

域名托管到 Cloudflare 之后，如果确实不需要 Workspace 的邮箱、文档等服务，可以按这个流程取消，只保留域名：

1. 登录 [admin.google.com](https://admin.google.com)，点击「结算」→「订阅」
2. 这里会看到两项：Workspace 订阅和域名注册。点击 **Workspace 订阅** 这一项进入详情页
3. 往下拉找到「更多」，点击进入，选择最下方的「取消订阅」
4. 按提示一路继续，选择取消原因，管理员邮箱填之前新建的那个账号
5. 看到取消成功的提示后，返回订阅列表，会看到只剩下「域名注册」这一项

取消后域名依然保留，续费也会按同样的价格自动扣费。

> 目前还没有验证过：改完 Nameserver 并取消订阅之后，长期来看是否会影响已经托管在 Cloudflare 上的域名解析。稳妥起见，建议取消订阅后观察几天，确认网站访问和 DNS 解析都正常，再放心用。

## 第七步：提前取消了订阅的——补救办法

如果你在托管到 Cloudflare 之前就已经取消了 Workspace 订阅，导致 Squarespace 那边验证不过，按这个办法解决：

1. 登录 Workspace 管理后台 [admin.google.com](https://admin.google.com)，用之前新建的账号登录
2. 进入「结算」→「订阅」，找一个最便宜的套餐重新订阅（比如商务新手版，价格通常在每月十几元人民币），走完下单流程
3. 订阅完成后，返回 Squarespace 后台，重新进入 DNS 下的 Domain Nameservers 项，这次账号验证就能正常通过了，按第四步的流程改 Nameserver 即可

------

## 常见问题

**Q：手机号验证一直过不去怎么办？**

A：实测发现用地址生成器生成的号码本身就能通过验证，且不需要接收验证短信，跟网上说的"IP不干净导致收不到验证码"不完全一致，可能只是格式校验。如果确实卡住，换个网络环境试试。

**Q：域名费用和 Workspace 服务费是一起扣的吗？**

A：不是，是两笔独立账单。域名费用（75里拉）下单当天就直接扣款；Workspace 服务如果选的是 Flexible plan，是按使用天数计提，攒到账单周期结束才统一出账扣款，短期在银行账户里看不到扣款记录属于正常现象。

**Q：改 Nameserver 时提示 `accounts.google.com is blocked` 怎么办？**

A：说明 Workspace 订阅已经被取消了，账号验证被拦。按上文「补救办法」重新订阅一个最便宜的套餐即可恢复验证。

**Q：这个域名以后续费还是这个价格吗？**

A：目前看是同价续费，但这是汇率套利，不是官方承诺的机制，Google 随时可能调整规则，但就算调整价格，也会与市面上的其他域名价格看齐，实在过高再转出也不迟。

**Q：能一次性续费好几年吗？**

A：目前普遍反馈是不支持批量续多年，只能一年一年正常续费。

------

## 总结

这套流程比正常买域名多绕了几步——先在 Workspace 下单，再到 Squarespace 完成验证和 DNS 设置，中间还有"订阅取消顺序"这种一不小心就会卡住的坑，但换来的是长期每年只要10块出头的续费成本。

如果你用这个域名建 Hugo 博客，接下来直接照着 [Hugo建站指南·初级02](/hugo-cloudflare-deploy/) 里「绑定自定义域名」那一节继续操作就行，后续流程完全一样。如果你更看重省心，不想折腾这几步跳转，Namesilo 或 spaceship 这类注册商首年续费透明、流程简单，也是很好的选择。
