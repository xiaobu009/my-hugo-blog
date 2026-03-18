---
title: "Cloudflare上托管SpaceShip域名的方法"
date: 2025-10-15
draft: false
description: "SpaceShip上购买域名，修正域名服务器无效，A记录解析异，禁用和启用DNSSEC托管域名的设置过程。"
url: "spaceship-domain-on-cloudflare"
categories:
    - 技术分享
tags:
    - 工具
    - 域名
---

{{< youtube yK9eI2yjuIk >}}

[ 【 **Youtube上观看** 】 ](https://youtube.com/watch?v=yK9eI2yjuIk)

>
Spaceship上购买了域名，由于DNSSEC设置错误，导致托管到Cloudflare时提示域名服务器无效，A记录解析异常无法获取免费证书。这里逐步展示禁用和启用DNSSEC，两种状态下托管域名的设置过程。
>

## 第一部分：Cloudflare上添加新域名

### 1.1、登录Cloudflare账户：

如果没有Cloudflare账户需要先注册一个，登录后点击【**加入域**】

![sc-001.webp](sc-001.webp)

### 2.1、添加域名到Cloudflare

输入在SpaceShip购买的域名，保持默认选择“快速扫描DNS记录”。继续。

![sc-002.webp](sc-002.webp)

### 2.2、选择免费计划

![sc-003.webp](sc-003.webp)

### 3、获取Cloudflare域名服务器地址：

这里CF会自动获取SpaceShip上的域名服务器信息，并完成A记录解析和NS解析。

点击“**继续前往激活**”

![sc-004.webp](sc-004.webp)

CloudFlare会提供两个名称服务器地址（**NS1和NS2**），设置时会将SpaceShip上的名称服务器地址，替换成这两个由CF提供的名称服务器地址。CF页面中也给出了操作提示。这里要特别注意这个“确保DNSSEC已关闭”的提示。许多小伙伴托管失败的原因就出在这里，DNSSEC设置下一步会详细说明。**未修改名称服务器之前，不要点【继续】**

![sc-006.webp](sc-006.webp)

## 第二部分：什么是DNSSEC

### DNSSEC说明

> DNSSEC是域名系统安全扩展，是DNS协议的扩展，用于增强域名系统（DNS）的安全性，防止数据篡改和欺骗攻击。禁用DNSSEC可简化管理或提高解析速度，但安全性会降低。启用DNSSEC显著提高DNS的安全性和可信度，但需要额外配置。
> 

**这里的DNSSEC设置有两种情况：**

一是禁用DNSSEC状态下托管域名

二是开启DNSSEC状态下托管域名

但不管是开启状态还是关闭状态，替换名称服务器这一步都是必须要做的。只是在DNSSEC设置上略有不同。具体操作下面会详细说明。

## 第三部分：禁用DNSSEC状态下托管域名

### 1、登录SpaceShi，进入Lanuchpad

![sc-007.webp](sc-007.webp)

**选择【高级DNS】**

![sc-008.webp](sc-008.webp)

![sc-009.webp](sc-009.webp)

如果域名是初次托管到CF上，CloudFlare端DNSSEC默认是关闭状态，这个设置无需任何改动。SpaceShip端默认是开启状态，目前我们就处在这个状态下。需提前关闭DNSSEC。

### 2、禁用Dnssec

点选要托管的域名

![sc-010.webp](sc-010.webp)

打开Dnssec选项卡，点下面的【**禁用**】按钮。弹出界面中点确定禁用Dnssec。此时会看到DNSSEC已禁用的提示。

![image.png](image.png)

![image.png](image-1.png)

![image.png](image-2.png)

### 3、修改名称服务器：

3.1、点击Dnssec上方区域中的【**更改**】按钮

![sc-012.webp](sc-012.webp)

3.2、弹出页面中，选【**自定义名称服务器**】删除原有的名称服务器

![sc-013.webp](sc-013.webp)

3.3、返回CloudFaare复制Cloudflare提供的两个名称服务器

![sc-014.webp](sc-014.webp)

3.4、将两个名称服务器地址，分别粘贴到下面的两个框中。

![sc-015.webp](sc-015.webp)

3.5、点【**保存DNS设置**】完成修改。提示域名服务器已更新

![sc-017.webp](sc-017.webp)

3.6、返回Cloudflare点【**继续**】让CF继续完成后续步骤。

![sc-018.webp](sc-018.webp)

之后看到CF提示需要24小时生效的提示，实际上不用等这么成时间，几分钟后返回Cloudflare。

![sc-019.webp](sc-019.webp)

看到域名状态显示为**活动**，说明域名已托管成功。到此，禁用DNSSEC状态下托管域名设置就完成了

![sc-020.webp](sc-020.webp)

## 第四部分：启用DNSSEC状态下托管域名

### 1、登录SpaceShi进入【Lanuchpad】

![sc-007.webp](sc-007-1.webp)

选择【**高级DNS**】

![sc-008.webp](sc-008-1.webp)

![sc-009.webp](sc-009-1.webp)

初次设置，SpaceShip的DNSSEC默认是开启状态，此时无需任何改动，点【明白了】继续下一步操作

### 2、点选要托管的域名

![sc-010.webp](sc-010-1.webp)

2.1、**点选要托管的域名，**点击【**更改**】按钮

![sc-012.webp](sc-012-1.webp)

2.2、选【**自定义名称服务器**】，已存在名称服务器，可添加新名称服务器，然后在删除原有的名称服务器

![sc-013.webp](sc-013-1.webp)

2.3、返回Cloudflare复制提供的两个名称服务器

![sc-014.webp](sc-014-1.webp)

2.4、分别粘贴到下面的两个框中

![sc-015.webp](sc-015-1.webp)

2.5、然后点【保存DNS设置】完成修改。提示域名服务器已更新。此时名称服务器添加完成

![sc-017.webp](sc-017-1.webp)

2.6、此时再次返回Cloudflare，查看域名状态，显示【名称服务器无效】，查看A记录解析状态，显示A记录【没有证书覆盖此主机名】。这是由于SpaceShip上已开启DNSSEC服务，但Cloudflare上还未开启，导致两边DNSSWEC设置不比配。接下来我们就去CF端开启DNSSEC服务。

![sc-021.webp](sc-021.webp)

![sc-022.webp](sc-022.webp)

### 3、Cloudflare中开启DNSSEC服务

在这之前SpaceShip通过直接的操作已经处于开启状态，现在需要将Cloudflare中DNSSEC服务业开启。

进入**Cloudflare账户主页** -> **点击域名** -> **DNS** -> **设置** -> **启用DNSSEC**

![sc-023.webp](sc-023.webp)

开启后会显示DNSSEC的配置参数，如果不小心关闭了这个窗口也不用担心，点右下方的【DS记录】就可以看到这些参数。

记录下摘要、摘要类型、算法和密钥标记这几个参数。后面在SpaceShip设置中要用到。

![sc-024.webp](sc-024.webp)

### 4、设置DNSSEC参数

返回SpaceShip->Lanuchpad->高级DNS页面->域名->Dnssec

点击【添加DS记录（Add DS record）】

![image.png](image-3.png)

将Cloudflare中记录的参数填入对应的位置保存即可。

![image.png](image-4.png)

> **SpaceShip与CloudFlare中DNSSEC参数对应关系参考下图**
> 

![sc-034.webp](sc-034.webp)

![sc-033.webp](sc-033.webp)

返回Cloudflare，等几分钟后Cloudflare中域名状态显示为活动，说明域名已托管成功

![image.png](image-5.png)

------

## **[ 实用工具 ]**

**1、自用VPN工具（PrivadoVPN）**：<br>
[https://s.ospace.top/PrivadoVPN](https://s.ospace.top/PrivadoVPN)<br>
零日志，瑞士隐私法保护，支持中文界面，**免费用户10G/月免费流量**，支持Talkatone注册和登录，
**支持** Windows，Android，macOS，ios，FireTV，AndroidTV，tvOS，Chrome**多种客户端**。
**付费用户：支持无限流量，无限设备**，67城市的服务器，最多 10 个设备同时连接，以及Socks5代理，广告拦截器，防病毒扫描等更多功能。
12+3个月：1**.33美金**/月，24+3个月：**1.11美金**/月，1个月计划：10.99美金/月。

**2、自用机场订阅Mitce**：<br>
[https://s.ospace.top/3tps6w](https://s.ospace.top/3tps6w)  **9折优惠码：**（**S4E6U9**）<br>
100GB/0.60美金/月、500GB/1.2美金/月、1000GB/2美金/月，不计量套餐/3美金，四款套餐可选，
包含住宅IP链路，支持多种客户端订阅，注册、养号、上网好帮手。


**3、Eskimo流量卡：**<br>
[https://s.ospace.top/mw9qyz](https://s.ospace.top/mw9qyz) 邀请码：**BD995** <br>
注册得**500MB**两年有效期的**免费全球数据流量**。
Eskimo是流量卡**不含号码**，支持**100多个国家**/地区漫游，从第一次激活使用流量开始计时，**长达2年有效期**，并且购买的流量**可转送其它Eskimo账户**。
购买中国区域流量或全球流量，在中国使用走**新加坡链路**，是新加坡**原生住宅IP**，非常适合申请国外应用及保号。

**4、ReadteaGO流量卡:** <br>
ReadteaGO链接: [https://esim.redteago.com/?c=i5oq82b3](https://esim.redteago.com/?c=i5oq82b3)<br>
ReadteaGO优惠码（5% 折扣）：**RTGF8F49L**

**5、域名注册Namesilo：**[https://www.namesilo.com](https://www.namesilo.com/?rid=f5e9423mw) ****（**oupons优惠码**：**092368xb** ）<br>
6、**SMS-Activate优惠链接**：[https://s.ospace.top/9tzyrx](https://s.ospace.top/9tzyrx)<br>
7、**Elevenlabs AI生成语音**：[https://try.elevenlabs.io/6xlgbhoqxkc8](https://try.elevenlabs.io/6xlgbhoqxkc8)