---
title: "NameSilo域名迁移到Spaceship的全过程＋省了多少年费？"
date: 2025-10-24
draft: false
description: "实测完整迁移流程，转移费用、转移时间，还有这么折腾到底能省多少钱?"
keywords: ["Namesilo 域名转移", "Spaceship 注册商", "域名平滑转出", "低成本便宜域名续费", "域名管理中心操作"]
url: "namesilo-domain-transfer-to-spaceship"
image: namesilo-domain-transfer-to-spaceship.webp
categories:
    - Technology
tags:
    - 工具
    - 网络
    - 域名
---

{{< youtube FCGh2YNWflM >}}

[ 【 **Youtube上观看** 】 ](https://youtube.com/watch?v=FCGh2YNWflM)

将NameSilo上多个域名迁移到SpaceShip上。视频会实测完整迁移流程，告诉你转移费用、转移时间，还有这么折腾到底能省多少钱。如果你也有域名迁移的想法，这个视频正好适合你。

整个过程分为四个部分（**1、注意事项  2、准备工作 3、转移步骤 4、成本计算**）

## 第一部分：注意事项

* 1、避免临近续费：域名到期前 15 天不要转移，可能导致续费问题。
* 2、检查锁定期：新注册域名 60 天内无法转移，确认域名已过此期限。
* 3、切换DNS ：转移后立即在 Spaceship 设置 DNS，避免网站中断。

## 第二部分：准备工作

### 1、确认域名解锁状态

登录 NameSilo，检查域名是否解锁。

路径：My Account → Domain Manager → 选择域名 → 确保 “Domain Lock” 为UNLOCKED状态。

![ns-008.webp](ns-008.webp)

### 2、检查 WHOIS 信息：

#### 2.1、确认邮箱可用

确保域名联系人邮箱可正常接收确认邮件。
查看位置：在 NameSilo 的 “WHOIS Privacy Settings” 中查看/更新邮箱。

修改位置：Account Settings → Personal Information → Email

#### 2.2、关闭WHOIS保护

路径：My Account → Domain Manager → 选择域名 → 确保 “WHOIS Privacy” 为OFF状态。

![ns-009.webp](ns-009.webp)

### 3、获取转移授权码（Authorization Code）

路径：My Account → Domain Manager → 自己的域名 → Authorization Code。 

点Send Email按钮，NameSilo会发一封包含授权码的邮件到注册邮箱。

![ns-010.webp](ns-010.webp)

![ns-011.webp](ns-011.webp)

## 第三部分：转移步骤

### 1、登录 Spaceship：

访问 [spaceship.com](http://spaceship.com/)，注册或登录账户。

![ns-012.webp](ns-012.webp)

### 2、发起转移

**2.1、输入转入域名**

点选 “Transfer a Domain ” → 输入域名 → 转移按钮。

![ns-013.webp](ns-013.webp)

**2.2、输入授权码。**

![ns-014.webp](ns-014.webp)

**2.3、确认状态**

输入授权码后，会看到状态不可用提示，这时需要点选一下空白的地方，让SpaceShip去查询一下域名的状态。如果在NameSilo中的设置 正确，这里会看到域名处于“已解锁”状态

![ns-015.webp](ns-015.webp)

**2.4、添加到购物车**

这里显示的价格多出了0.2美元，是因为无论注册、转移还是续费，ICANN都会强制收取0.2美元的费用。我们之所以很少看到，是因为域名注册商通常会将这个费用包含在注册、转移及续费的费用中，并未提及。而SpaceShip是将这个费用直接展示在官网及注册过程中了。点击结账跳转到支付页面。

![ns-016.webp](ns-016.webp)

**2.5、选择支付方式并添加联系人信息**

SpaceShip支持信用卡，借记卡，PayPal，支付宝，Google Pay等多种支付方式，如果是为SpaceShip账户充值，还支持比特币，Pllaid银行转账（这个仅限美国银行）

![ns-017.webp](ns-017.webp)

这里注意，SpaseShip平台要求注册人购买域名时，必须填写联系人地址。如果注册时已填写这里直接选择即可。如果未添加这里可按照提示添加联系人信息。

![ns-018.webp](ns-018.webp)

![ns-019.webp](ns-019.webp)

**2.6、支付**

点击立即付款完成支付。支付成功后会看到“感谢您选择SpaceShip”的提示。完成支付后，域名有效期会在原有的基础上自动延长一年获多年，这个根据实际购买的年限而定。点管理跳转到域名转移状态页面

![ns-020.webp](ns-020.webp)

**2.7、查看域名转移状态**

这里我们能看到提示，域名将在5天内自动转入SpaceShip。如果不着急等几天即可。如果想马上转入，看下面的操作

![ns-021.webp](ns-021.webp)

**2.8、手动确认域名转移**
返回NameSilo。在Transfer Manager域名转移管理页面中，最下方能看到正在转移的域名，这里可进行手动确认。路径：My Account → Transfer Manager → 最下面。

![ns-022.webp](ns-022.webp)

![ns-023.webp](ns-023.webp)

点击最右侧的编辑按钮，再点击SUBMIT按钮确定即可。

![ns-024.webp](ns-024.webp)

![ns-025.webp](ns-025.webp)

当确认成功后会看到15分钟内容完成请求的提示。

![ns-026.webp](ns-026.webp)

稍等片刻返回SpaceShip，刷新域名转移管理页面，会看到域名已经成功转入提示。同时注册邮箱会收到一封SpaceShip发送的，域名转移成功通知邮件。

![ns-027.webp](ns-027.webp)

![ns-028.webp](ns-028.webp)

## 第四部分：成本计算

接下来我们看看这么折腾一下后，能省多少钱。

下面是NameSilo上一些域名的价格，黑色加粗的是目前购买价格，红色带横线的是第二年续费价格，这个价格已包含了ICANN的费用。

![ns-002.webp](ns-002.webp)

下面这个这个是SpaceShip上域名的价格，黑色加粗的是目前购买价格，灰色带横线的是第二年续费价格，这里显示的价格未包含ICANN域名转移的手续费0.2美元。

![ns-003.webp](ns-003.webp)

![ns-029.webp](ns-029.webp)

加上后SpaceShip中TOP域名续费价格4.05，比NameSilo便宜了0.83美元，COM域名续费价格9.98，比NameSilo便宜了7.13美元，ONE域名续费价格19.67，比NameSilo便宜了5.08美元。

![ns-007.webp](ns-007.webp)

目前小布这个NameSilo账号中，有3个COM域名，1个O-R-G域名，一个ONE域名，4个TOP域名，1个XYZ域名，总共10个域名，NameSilo中的年续费加一起是107.92美元，转移到SpaceShip后省了30.84美元，整体成本降低了28.5%。真是不算不知道一算吓一跳，还是挺多的。

小伙伴如果也有这种情况，那也快去试试吧。

=============================================

## [ 实用工具 ]

**1、自用VPN工具（PrivadoVPN）**[https://s.ospace.top/PrivadoVPN](https://s.ospace.top/PrivadoVPN)<br>
零日志，受瑞士隐私法保护，支持中文界面，每月10G免费流量，支持Talkatone注册和登录，
支持：Windows，Android，macOS，ios，FireTV，AndroidTV，tvOS，Chrome等多种客户端。
付费用户：支持无限流量，无限设备，67城市的服务器，最多 10 个设备同时连接，以及Socks5代理，广告拦截器，防病毒扫描等更多功能。
12+3个月：1.33美金/月，24+3个月：1.11美金/月，1个月计划：10.99美金/月。

**2、自用机场订阅Mitce**：<br>
[https://s.ospace.top/3tps6w](https://s.ospace.top/3tps6w)  **9折优惠码：**（**S4E6U9**）<br>
100GB/0.60美金/月、500GB/1.2美金/月、1000GB/2美金/月，不计量套餐/3美金，四款套餐可选，
包含住宅IP链路，支持多种客户端订阅，注册、养号、上网好帮手。


**3、Eskimo流量卡：** <br> 
[https://s.ospace.top/mw9qyz](https://s.ospace.top/mw9qyz)  **邀请码：BD995**<br>
注册得500MB两年有效期的免费全球数据流量。
Eskimo是流量卡不含号码，支持100多个国家/地区漫游，从第一次激活使用流量开始计时，长达2年有效期，并且非免费赠送流量可转送到其它Eskimo账户。
购买中国区域流量或全球流量，在中国使用走的是新加坡网络链路，获取的是新加坡的原生住宅IP，非常适合申请国外应用及保号。

**4、ReadteaGO流量卡:**<br>
ReadteaGO链接: [https://esim.redteago.com/?c=i5oq82b3](https://esim.redteago.com/?c=i5oq82b3)<br>
ReadteaGO优惠码（5% 折扣）：**RTGF8F49L**

**5、域名注册Namesilo：**[https://www.namesilo.com](https://www.namesilo.com/?rid=f5e9423mw) ****（**oupons优惠码**：**092368xb** ）<br>
**6、SMS-Activate优惠链接**：[https://s.ospace.top/9tzyrx](https://s.ospace.top/9tzyrx)<br>
**7、Elevenlabs AI生成语音**：[https://try.elevenlabs.io/6xlgbhoqxkc8](https://try.elevenlabs.io/6xlgbhoqxkc8)