---
title: "Google VPN流量共享给电脑的方法"
date: 2025-12-22
draft: false
description: "如何在PC使用Pixel手机上的Google-VPN流量"
url: "google-vpn-traffic-sharing"
categories:
    - 科学上网
tags:
    - 工具
    - 网络
---

{{< youtube OMTlT0MrK18 >}}

[ 【 **Youtube上观看** 】 ](https://youtube.com/watch?v=OMTlT0MrK18)

## 工具链接

* **PrivadoVPN：**[https://s.ospace.top/PrivadoVPN](https://s.ospace.top/PrivadoVPN)
* **v2rayN：**[https://github.com/2dust/v2rayN/releases](https://github.com/2dust/v2rayN/releases)
* **Cloudflare WARP：**[https://one.one.one.one/zh-Hans/](https://one.one.one.one/zh-Hans/)
* **Eskimo流量卡(邀请码：BD995 )：**[https://s.ospace.top/mw9qyz](https://s.ospace.top/mw9qyz)
* **IP检测工具：**[https://ip.sb](https://ip.sb/)
* **IP纯净度检测工具：**[https://scamalytics.com/](https://scamalytics.com/)

> 谷歌Pixel手机上隐藏着一款，免费，不限流量，ip纯净度极高的VPN应用，它就是谷歌自组研发的VPN工具→**Google VPN。**本期视频主要介绍PC端怎么使用Pixel手机上的Google VPN流量。
> 

## 第一部分：Pixel手机端代理工具获取及设置

### 1、Every Proxy工具获取

打开谷歌应用市场，搜索 **every proxy** 安装即可。

![](v-008.webp)![](v-009.webp)![](v-010.webp)

**网络环境搭建方法：**

无法访问谷歌应用市场的小伙伴，看看这些网络环境搭建分享。

> 
* **1-PrivadoVPN：**[https://www.smallstep.one/article/talkatone-tools](https://www.smallstep.one/article/talkatone-tools)
* **2-Cloudflare团队账户：**[https://www.smallstep.one/article/cloudflare-1](https://www.smallstep.one/article/cloudflare-1)
* **3-自建机场节点：**[https://www.smallstep.one/article/aws-vps](https://www.smallstep.one/article/aws-vps)
* **4-Eskimo流量卡：**[https://www.smallstep.one/article/eskimo.html](https://www.smallstep.one/article/eskimo.html)
* **5-ESTK的安装与使用：**[https://www.smallstep.one/article/estk-card-setup.html](https://www.smallstep.one/article/estk-card-setup.html)
> 

### 2、Every Proxy工具说明

**Every Proxy**提供了两种代理方式，一是**Http代理**，二是**Socks代理**，使用时可开启其中任意一个或全部开启均可。
**Http Proxy 包含3个或4个IP地址**和一个端口。同样**Socks Proxy**代理中也**包含相同的IP地址**，**唯一的区别是端口不同**，**Http Proxy中是8080端口**，**Socks Proxy中是1080端口**。端口可以修改但通常并不需要，保持默认即可，要想修改可点编辑图标进入即可修改。这

![Http+Socks Proxy](v-011.webp)![Http Proxy 地址](v-012.webp)![Socks Proxy地址](v-013.webp)![修改端口](v-014.webp)

## 第二部分：谷歌Pixel手机流量共享的四种方式

谷歌手机提供了四种共享流量的方法，一是通过**WLAN热点**共享，二是通过**USB网络**共享，三是通过**蓝牙**网络共享，四是通过**以太网络**共享。

**开启路径**：【设置】→【网络和互联网】→【热点和网络共享】

![](v-016.webp)![](v015.webp)![](v017.webp)

### 一：通过WLAN热点共享手机流量

这种是通过手机的无线热点将流量共享给其他客户端，也是最常用的方式，比较**适合**手机、笔记本电脑这些**拥有Wifi或无线网卡的设备**。如果是台式机**没有无线网卡**的情况，可参**照后面的三种设置方式**。

#### 1、开启Google VPN，开启Every Proxy代理

**Google VPN开启路径**：【设置】→【网络和互联网】→【VPN】

![](v-019.webp)![](v-018.webp)![](v-020.webp)

#### 2、开启谷歌手机热点共享

**开启路径：**【设置】→【网络和互联网】→【热点和网络共享】→点击【WLAN热点】

![](v-021.webp)![](v-022.webp)![](v-023.webp)

#### 3、链接谷歌手机热点

点开笔记本电脑右下角的网络，找到Pixel手机提供的热点名称，输入登录密码完成Wifi链接设置

![](v-024.webp)
![](v-025.webp)
![](v-026.webp)

#### 4、电脑端代理设置

电脑端的代理设置有**两种情况**，**一是Windows系统本地代理**方式，**二是使用第三方的代理工具**

**4.1、Windows系统本地代理方式**

**设置路径：** 右下角【视窗图标】→【设置】→【网络和Internet】→【代理】→【手动设置代理】

![v-027.webp](v-027.webp)

**4.2、使用第三方代理工具**

打开**v2rayN**代理工具，点左上角的配置文件，选择添加【Http】配置文件。

![v-028.webp](v-028.webp)

别名任意，地址就是Every Proxy提供Http地址，端口是8080，保存。

![v-029.webp](v-029.webp)

然后再用同样的方法，将【Socks】的链接添加上，别名任意，这里填写Every Proxy提供的Socks地址，端口是1080。

![v-031.webp](v-031.webp)

添加完成后我们测试一下真链接，有数值说明链接成功了。

![v-032.webp](v-032.webp)

这里提醒一下，每次启动Every Proxy时地址可能会变化，注意确认一下。

![v-033.webp](v-033.webp)

### 二：通过USB网络共享手机流量

#### 1、准备链接线

因为是通过USB网络共享，所以这里要用到一根TypeC转USB的线，并将USB线TypeC端链到手机的TypeC口，另一端链接到电脑USB口。

![3db60b6ba65c50a1.webp](3db60b6ba65c50a1.webp)

![Firefly_Gemini Flash_A clean, step-by-step instructional illustration in realistic style, horizontal 1920x 974616.webp](Firefly_Gemini_Flash_A_clean_step-by-step_instructional_illustration_in_realistic_style_horizontal_1920x_974616.webp)

![piclumen-1767962766054.webp](piclumen-1767962766054.webp)

USB线链接到电脑后，windows系统会自动识别安装驱动，此时USB口会以网卡的形式存在。

![v-034.webp](v-034.webp)

#### 2、开启USB网络共享

**开启路径：**【设置】→【网络和互联网】→【热点和网络共享】→【USB网络共享】

![v-035.webp](v-035.webp)

#### 3、设置代理

添加Http及Socks链接的方法与WLAN共享添加方式相同

![v-029.webp](v-029-1.webp)

### 三：通过蓝牙网络共享手机流量

#### 1、开启蓝牙共享：

**开启路径：**【设置】→【网络和互联网】→【热点和网络共享】→点击【蓝牙网络共享】

![v-036.webp](v-036.webp)

#### 2、蓝牙配对过程：

**设置路径1：**【右下角箭头】→【蓝牙】→【添加蓝牙设备】

![v-037.webp](v-037.webp)

**设置路径2：点【添加蓝牙或其他设备】

![v-038.webp](v-038.webp)

**设置路径3：选【鼠标、键盘、手写笔、…】

![v-039.webp](v-039.webp)

**设置路径4：选【鼠标、键盘、手写笔、…】

选**【**鼠标、键盘、手写笔、…】后，系统自动会搜索附近的蓝牙设备，有时会显示蓝牙设备的名称，有时显示的是【**未知设备**】，根据自己本地情况选择即可，选对了会在Pixel端看到配对信息。点击【配对】确认。

![v-040.webp](v-040.webp)

![v-041.webp](v-041.webp)

![](v-042.webp)![](v-043.webp)

完成上面手机端【配对】确认后，回到电脑端点【链接】完成配对

![v-041.webp](v-041.webp)

完成配对后，会看到Pixel手机名称

![v-044.webp](v-044.webp)

#### 3、代理设置

**添加Http及Socks链接**，添加方法与WLAN共享添加方式相同

![v-029.webp](v-029-1.webp)

### 四：通过以太网络共享手机流量

#### 1、准备网线和转换接头

由于此步骤需要手机与以太网对接，所以设置前需要准备转换接头及制作好的网线。转换接头一端是TypeC口另一端是以太网口。Type-C端接谷歌手机，以太网口接网线，网线的另一端接电脑或交换机。

![890f4cbf-e0d5-4f22-be6a-283c41bb81d0.webp](890f4cbf-e0d5-4f22-be6a-283c41bb81d0.webp)

![4ccd78af-370f-4be1-9dd5-23ef72dfd6f3.webp](4ccd78af-370f-4be1-9dd5-23ef72dfd6f3.webp)

#### 2、开启以太网络共享

**开启路径：**【设置】→【网络和互联网】→【热点和网络共享】→【以太网络共享】

灰色表示未连接，变亮表示链接成功

![未连接](v-045.webp)![链接成功](v-046.webp)![开启以太网共享](v-047.webp)

#### 3、连接以太网

网络链接有两种方式：一种是网线**直连电脑网卡**，另一种是网线**直连交换机**
**方式一：**转换头TypeC端接Pixel手机，以太网口端接网线，网线的另一端直连到电脑的网口。

![bbc64692-4031-4427-a070-f855e50b35ca.webp](bbc64692-4031-4427-a070-f855e50b35ca.webp)

**方式二：**转换头TypeC端接Pixel手机，以太网口端接网线，网线的另一端直连到交换机。

![4ccd78af-370f-4be1-9dd5-23ef72dfd6f3.webp](4ccd78af-370f-4be1-9dd5-23ef72dfd6f3-1.webp)

#### 4、代理设置

添加Http及Socks链接的方法与WLAN共享添加方式相同

![v-029.webp](v-029-2.webp)

至此我们分享了四种Pixel8a流量共享的方法。如果有无线网卡使用Wlan的方式，没有无线网卡可选USB、以太网方式，具体使用种方式看实际情况和自己的需要。

以上的设置过程，是以微软的windows系统为蓝本制作的，如果在Mac系统上设置，可以采用WLAN热点共享的方式，设置方法与Win下设置类似，先链接手机Wifi，再设置代理即可。

## [ 实用工具 ]

* **1、自用VPN工具（PrivadoVPN）**：[https://s.ospace.top/PrivadoVPN](https://s.ospace.top/PrivadoVPN)<br>
零日志，瑞士隐私法保护，支持中文界面，**免费用户10G/月免费流量**，支持Talkatone注册和登录，
**支持** Windows，Android，macOS，ios，FireTV，AndroidTV，tvOS，Chrome**多种客户端**。
**付费用户：支持无限流量，无限设备**，67城市的服务器，最多 10 个设备同时连接，以及Socks5代理，广告拦截器，防病毒扫描等更多功能。
12+3个月：1**.33美金**/月，24+3个月：**1.11美金**/月，1个月计划：10.99美金/月。
* **2、自用机场订阅Mitce**：[https://s.ospace.top/3tps6w](https://s.ospace.top/3tps6w)<br>
100GB/0.60美金/月、500GB/1.2美金/月、1000GB/2美金/月，不计量套餐/3美金，四款套餐可选，
包含住宅IP链路，支持多种客户端订阅，注册、养号、上网好帮手。
**9折优惠码：**（**S4E6U9**）
* **3、Eskimo流量卡：**[https://s.ospace.top/mw9qyz](https://s.ospace.top/mw9qyz)<br>
邀请码：**BD995** 得**500MB**两年有效期的**免费全球数据流量**。
Eskimo是流量卡**不含号码**，支持**100多个国家**/地区漫游，从第一次激活使用流量开始计时，**长达2年有效期**，并且购买的流量**可转送其它Eskimo账户**。
购买中国区域流量或全球流量，在中国使用走**新加坡链路**，是新加坡**原生住宅IP**，非常适合申请国外应用及保号。
* **4、ReadteaGO流量卡:** <br>
ReadteaGO链接: [https://esim.redteago.com/?c=i5oq82b3](https://esim.redteago.com/?c=i5oq82b3)
ReadteaGO优惠码（5% 折扣）：**RTGF8F49L**
* **5、域名注册Namesilo：**[https://www.namesilo.com](https://www.namesilo.com/?rid=f5e9423mw) ****（**oupons优惠码**：**092368xb** ）
* **6、SMS-Activate优惠链接**：[https://s.ospace.top/9tzyrx](https://s.ospace.top/9tzyrx)
* **7、Elevenlabs AI生成语音**：[https://try.elevenlabs.io/6xlgbhoqxkc8](https://try.elevenlabs.io/6xlgbhoqxkc8)