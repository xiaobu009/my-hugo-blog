---
title: "[免费科学上网-1] 不绑信用卡申请Cloudflare团队账户（Zero Trust）"
date: 2023-10-07
draft: false
description: "通过设置Cloudflare的Zero Trust账户，获取无限流量，免费科学上网，看油管，看奈菲，使用Midjourney等应用"
url: "cloudflare-1"
categories:
    - 科学上网
tags:
    - 网络
---

<aside>
😀 当我们想看油管、看奈菲、想使用Midjourney等应用的时候，却发现这些网站已被屏蔽根本就无法访问。于是使用VPN软件来解除限制，却发现种类多、设置复杂、而且不便宜。如果您想免费实现科学上网，今天这篇文章一定不要错过。

</aside>

{{< youtube qyufPce-00s >}}

[ 【 **Youtube上观看** 】 ](https://youtube.com/watch?v=qyufPce-00s)

Cloudflare上网方式有三种。1、warp账户，默认开通送1G流量。2、warp+账户，相当于warp pro账户，通过关注telegram相关频道可获得23.84PB流量，这是多少？我给大家简单换算一下，1G等于1千兆，1T等于1千G，1PG等于1千T）这些浏流量对于普通用户来说已经足足够用了，但还有比这更好的方案，那就是使用团队账户上网，如果开通了teams团队账户（Zero Trust账户），就相当于获得了无限的流量。今天我们就介绍这个最强方式，通过开通团队账户免费科学上网。

## 第一步：注册Cloudflare账户

Cloudflare注册对区域，邮箱没有要求，国内的QQ邮箱、163邮箱都可注册Cloudflare账户，但得是真实的邮箱，能够接收验证邮件。

### 打开Cloudflare官网：

[https://www.cloudflare.com/zh-cn/](https://www.cloudflare.com/zh-cn/)

![Untitled](Untitled.webp)

### 输入邮箱和密码

![Untitled](Untitled1.webp)

### 验证邮箱：

![Untitled](Untitled2.webp)

## 第二步：Zero Trust 设置

### 登录CF

登录后，点击Zero Trust开始进入设置

![Untitled](Untitled3.webp)

### 输入团队名称

团队名称可任意起名，系统会验证是否重名。注册Cloudflare后第一次登陆，进入Zero Trust时会看到下面的页面。

![Untitled](Untitled4.webp)

如果已经添加过团队名称会看到下面的页面

![Untitled](Untitled5.webp)

### 选择免费计划

输入团队名称后，点击Next进入付费计划选择页面，选免费计划（Free $0/user/month）。

![Untitled](Untitled6.webp)

![Untitled](Untitled7.webp)

### 选择继续支付

（proceed to payment）。这里不用担心，由于我们选择的是免费计划，输入信用卡信息点击支付时不会扣费。免费计划支持50个用户登陆访问。

![Untitled](Untitled8.webp)

### 添加支付方式

点 Add payment method 按钮进入支付信息填写页面

![Untitled](Untitled9.webp)

下面的步骤需要绑定信用卡，想绑定信用卡的朋友接着往下看，不想绑定信用卡的朋友跳到【**第五步**】

### 添加支付信息

信用卡需使用可以支付外币的信用卡，比如带VISA/MASTER标识的信用卡卡。

用户名称用汉语拼音填写，与信用卡一致，地址信息照实填写就行，也可以找一个虚拟地址。

![Untitled](Untitled10.webp)

![Untitled](Untitled11.webp)

### 点下一步支付

这里我们可以看到，费用是零。这里也可看到，团队用户计划中，可以允许50个用户登陆访问。

![Untitled](Untitled12.webp)

## 第三步：设置客户端验证规则

### Settings进入客户端设置

点击Settings，再点WARP Client

![Untitled](Untitled13.webp)

### 点击Manage进入客户端规则设置

注意：只有绑定了支付信息的用户，才能看到WARP Client下的Manage按钮。

![Untitled](Untitled14.webp)

### 点击Add a rule添加规则

在这里添加的规则面对的是想登录的客户端用户

![Untitled](Untitled15.webp)

### 设置用户登录规则

**Rule name：**规则名称，名称可以任意填写

**Rule action：**规则动作，保持默认（**Allow**）

**Selector：**选择约束条件，这里选择以邮箱后缀最为约束条件（**Emails ending in**）

**Value：**允许登录的邮箱后缀。如下图中添加了四个邮箱后缀，表示只有这四类邮箱才可以注册登录。

**Save：**添加完成后，一定要记得保存

![Untitled](Untitled16.webp)

## 第四步：下载客户端

Cloudflare支持五种客户端，macOS、windows、iOS、Android、Linux

点Downloads进入客户端下载页面。也可打开 [https://1.1.1.1](https://1.1.1.1) 页面下载

![Untitled](Untitled17.webp)

![Untitled](Untitled18.webp)

## 第五步：无卡注册步骤

### 链接设备

从支付信息填写页面退出后，点击My Team下的Devices调出Connect a device 按钮。第一次进入时看不到Connect a device 按钮，需按照下面方法操作。

**操作方法：**先点击My Team下的Users，再点击Devices，如果没有出现，再次按顺序点Users和Devices，直到Devices下出现Connect a device 按钮，如下图所示。

![Untitled](Untitled19.webp)

### 添加验证规则

点击【 **Connect a device 】**打开添加登陆规则页面，点击【**Create an enrollment policy**】按钮

![Untitled](Untitled20.webp)

### 输入登陆规则

系统默认以邮箱后缀作为客户端的验证规则。这里填写需要验证的邮箱后缀（如：@hotmail.com），然后点 save 保存。

![Untitled](Untitled21.webp)

### 获取团队名称

点击页面中的 Windows按钮，系统会自动打开一个页面，不用管。

![Untitled](Untitled22.webp)

![Untitled](Untitled23.webp)

### 复制团队名称

**5.1、点击选项卡，返回之前的页面**

![Untitled](Untitled24.webp)

**5.2、返回后会看到团队名称。复制团队名称，复制完成后**这个页面不要关闭，一直保持这个状态**。**

![Untitled](Untitled25.webp)

## 第六步：客户端设置

### 安装客户端

此处以windwos版安装为例。除了Win版，Cloudflare还提供了macOS、安卓版、苹果版、Linux版。

安装非常简单，双击运行安装文件，按照提示点击下一步完成安装即可。

![Untitled](Untitled26.webp)

![Untitled](Untitled27.webp)

![Untitled](Untitled28.webp)

安装完成后，会在右下角看到一个类似云朵的图标。点击右下角的Cloudflare图标，按照提示点击下一步，并同意并接受协议。

![Untitled](Untitled29.webp)

![](Untitled30.webp)![](Untitled31.webp)

### 设置团队账户（Zero Trust）

**2.1、点击右下角Cloadflare图标，点击齿轮图标**

![Untitled](Untitled32.webp)

**2.2、添加偏好设置**

![Untitled](Untitled33.webp)

**2.3、点击账户→ 使用Cloudflare Zero Trust 登陆**

![Untitled](Untitled34.webp)

**2.4、点下一步并同意隐私条款**

![](Untitled35.webp)![](Untitled36.webp)

**2.5、输入团队名称并确定**

![Untitled](Untitled37.webp)

**2.6、发送验证码**

输入要登陆的邮箱地址后，点击发送按钮（send me a code），系统会发一串验证码到这个邮箱。

![Untitled](Untitled38.webp)

**2.7、接受验证码并验证**

![Untitled](Untitled39.webp)

![Untitled](Untitled40.webp)

**验证成功后**

![Untitled](Untitled41.webp)

**点击右下角的Cloadflare图标，稍等片刻后，图标由红色的warp+改为蓝色的Zero Trust，注册完成。**

![Untitled](Untitled42.webp)

**点击按钮后显示已连接，表示已经成功联网，可以科学上网了。**

![Untitled](Untitled43.webp)

## 绑定信用卡与未绑定的不同

绑定信用卡后，登陆账户可以随时编辑、修改、删除客户端登陆规则。未绑定信用卡的用户登陆后，无法看到已有的规则、虽然能添加规则，但无法修改和删除规则，添加规则操作不直观也不方便，操作相对繁琐。

## 视频教程：[YouTube](https://youtu.be/qyufPce-00s)

{{<youtube qyufPce-00s>}}

[ 【 **Youtube上观看** 】 ](https://youtube.com/watch?v=qyufPce-00s)