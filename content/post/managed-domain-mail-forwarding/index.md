---
title: "Cloudflare上托管域名并设置邮件转发"
date: 2023-08-18
draft: false
description: "Cloudflare上托管域名并设置邮件转发服务，来获得拥有自己域名后缀的邮箱"
url: "managed-domain-mail-forwarding"
categories:
    - 技术分享
tags:
    - 域名
    - Email
---

<aside>
😀 网络中有许多平台提供免费邮箱申请，虽然免费，但无法自定义域名后缀。虽然某些平台提供免费试用，但都有时间限制，要么直接购买邮件服务，但需要付费。现在我们可以利用Cloutflare提供的电子邮件路由功能，设置多个拥有自己域名后缀的免费邮件转发服务。重点是它免费。

</aside>

# 实现目标：

NameSilo上申请域名，申请Cloudflare账号，更改NameSlio新域名的DNS解析，Cloudflare上托管域名设置，Cloudflare上设置以新域名结尾的邮箱，并转发邮件。

# 设置前准备
注：以下的选项可以在无需科学上网的情况下完成所有操作
1. 一个可以正常使用的邮箱（Outlook邮箱）
2. Cloudflare账号（可用Outlook申请）
3. NameSilo域名申请平台账号（可用Outlook申请）

# 操作步骤：
1. 申请域名
2. 注册 [NameSilo](https://www.namesilo.com/?rid=f5e9423mw) 账号
3. 注册 [Cloudflare](https://www.cloudflare-cn.com/) 账号
4. 设置域名托管
5. 设置邮件转发

## **1、申请域名**

### **1.1、打开网址：**

[**NameSilo](https://www.namesilo.com/?rid=f5e9423mw) 搜索想要的域名**

选择NameSilo是因为没有套路，价格透明实惠，可以使用支付宝，无需外币卡。

![Untitled](Untitled.webp)

### **1.2、添加域名到购物车**

（蓝色表示可以注册，黄色标识已经被注册，点**Add**添加到购物车）

这里最便宜的域名只要1.88美金（第一年），如果输入了优惠码1美金就能买到（同时关注续费价格，选择后期续费（Renewal）便宜的，这里最便宜的续费价格是4.88美金）

![Untitled](Untitled.png)

### **1.3、点 Checkout 注册缴费**

![Untitled](Untitled-1.webp)

## **2、注册NameSilo账号**

### **2.1、输入注册信息**

![Untitled](Untitled-2.webp)

### **2.2、验证邮箱**

注册提交后NameSilo会发一封邮件到注册邮箱，需要点击激活链接激活邮箱。

![Untitled](Untitled-3.webp)

### **2.3、进入购物车页面**

![Untitled](Untitled-4.webp)

**2.4、提交订单**

输入优惠码省1美金（**092368xb**），优惠码只对新账号有效 ，点击 **CHECKOUT** 进行支付

![Untitled](Untitled-5.webp)

### **2.5、选择支付宝**

![Untitled](Untitled-6.webp)

![Untitled](Untitled-7.webp)

![Untitled](Untitled-8.webp)

![Untitled](Untitled-9.webp)

支付成功后系统返回到Dashboard页面

![Untitled](Untitled-10.webp)

点击 Account Settings 添加用户信息

![Untitled](Untitled-11.webp)

用户信息根据自己的情况填写即可，填写完成后点【Save】保存提交

![Untitled](Untitled-12.webp)

## **3、注册 Cloudflare账号**

**注册非常简单，三步即可完成**

### **3.1、打开 [Cloudflare](https://www.cloudflare.com) 网站，修改语言**

![Untitled](Untitled-13.webp)

![Untitled](Untitled-14.webp)

### **3.2、输入账号密码，提交注册**

![Untitled](Untitled-15.webp)

![Untitled](Untitled-16.webp)

### **3.2、验证邮箱：**

![Untitled](Untitled-17.webp)

## **4、域名托管**

### **4.1、添加站点**

![Untitled](Untitled-18.webp)

**输入域名后点击【添加站点】**

![Untitled](Untitled-19.webp)

**选择【Free】计划，并点【继续】**

![Untitled](Untitled-20.webp)

**添加更多DNS页面点【继续】**（DNS解析记录可以后添加）

![Untitled](Untitled-21.webp)

**弹出提示“以后添加记录”，点【确定】**

![Untitled](Untitled-22.webp)

**系统转入更改名称服务器信息页面**

![Untitled](Untitled-23.webp)

![Untitled](Untitled-24.webp)

**Cloudflare在此处提供了域名服务器的修改方法：**

**1、**到域名注册服务商平台，删除以下名称服务器：

ns1.dnsow1.com

ns2.dnsow1.com

ns3.dnsow1.com

**2、**添加以下两条友Cloudflare提供的名称服务器

NameServer 1：anahi.ns.cloudflare.com

NameServer 2：dan.ns.cloudflare.com

**下面将进入到域名服务器更改设置，会用到上面的参数**

### **4.2、登录NameSilo，修改域名服务器设置**

**第一步：点击Dpmain Manager 进入域名列表**

![Untitled](Untitled-25.webp)

**第二步：点击域名右侧的三个圆盘进入域名服务器设置**

![Untitled](Untitled-26.webp)

如果看到是红色的卡车，说域名申请后还未配置好，需要多等一下

![Untitled](Untitled-27.webp)

**第三步：将下面三个参数**

ameServer 1：**ns1.dnsow1.com**

ameServer 2：**ns2.dnsow1.com**

ameServer 3：**ns3.dnsow1.com**

**并替换为：**

NameServer 1：**anahi.ns.cloudflare.com**

NameServer 2：**dan.ns.cloudflare.com**

![Untitled](Untitled-28.webp)

![Untitled](Untitled-29.webp)

**第四步：提交：**

![Untitled](Untitled-30.webp)

至此NameSilo平台域名设置部分已经完成，

**第五步：回到Cloudflare平台，继续完成名称服务器设置：**

![Untitled](Untitled-31.webp)

![Untitled](Untitled-32.webp)

![Untitled](Untitled-33.webp)

![Untitled](Untitled-34.webp)

看到【**有效**】提示后，说明域名托管已成功，今后有关这个域名的解析都会在Cloudflare平台上完成

![Untitled](Untitled-35.webp)

**第六步：配置域设置，提高安全性及性能。**

**点击概述-》查看设置**

![Untitled](Untitled-36.webp)

**点击【开始使用】**

![Untitled](Untitled-37.webp)

![Untitled](Untitled-38.webp)

![Untitled](Untitled-39.webp)

![Untitled](Untitled-40.webp)

![Untitled](Untitled-41.webp)

至此域名托管设置已经完成。

## **5、设置邮件转发**

市面上有许多平台可以提供免费的邮箱，但无法使用自己的域名作为后缀。如果要使用自己的域名后缀设置邮箱，要么有使用时间限制，要么收费。现在，我们可以使用Cloutflare提供免费电子邮件路由功能，设置一个或多个拥有自己域名后缀的免费邮件转发服务，并且可以做的多个不同的邮件地址对应不同的真实邮箱。

设置邮件路由的前提是，电子邮件后缀域名已经托管到Cloudflare上。

### **5.1、设置域名**

**点击主页下面需要设置电子邮件路由服务的域名，进入此域名的设置页面**

![Untitled](Untitled-42.webp)

### **5.2、输入新邮箱名称及要接收转发邮件的邮箱**

自定义地址：指的是要建立的新邮箱名称（可随便起名）

目标位置：指的是背后要接收转发邮件的邮箱，即真实的邮箱（可以是任意可用的邮箱），如果是第一次设置目标位置，直接填写真实邮箱即可。

![Untitled](Untitled-43.webp)

### **5.3、启用设置**

![Untitled](Untitled-44.webp)

### **5.4、设置成功**

**路由状态显示【已启动】说明设置完成，可以用设置的邮箱转发邮件了。**

![Untitled](Untitled-45.webp)

### **5.5、创建新路由地址**

![Untitled](Untitled-46.webp)

### **5.6、在【路由】页面下方添加更多的【目标地址】**（真实的收件地址）

![Untitled](Untitled-47.webp)

![Untitled](Untitled-48.webp)