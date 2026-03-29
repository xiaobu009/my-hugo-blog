---
title: "[免费科学上网-2] 部署Workers脚本，免费科学上网"
date: 2023-10-26
draft: false
description: "通过workers将vless部署到Cloudflare中,无障碍访问奈菲 ChatGPT"
keywords: ["Cloudflare Workers 开发", "免费节点代理一键部署", "边缘节点计算平台", "无服务器微代理应用", "轻量化反向代理设置教程"]
url: "workers-deployment"
image: free-vpn2-workers-deploy.webp
categories:
    - Internet
tags:
    - 网络
---

<aside>
当我们建立Cloudflare团队账户后，就打通了外网访问路径，实现了免费科学上网，但您是不是发现仍然不能正常访问ChatGPT，一会能访问一会又不能了，而且网速还很慢。今天通过对workers脚本的部署，手把手带您创建一个能无障碍访问油管、奈菲，chatgpt的科学上网环境。如果您还缺一个免费科学上网的备选方案，并且想了解worker脚本的部署方法，一定不要错过。

</aside>
{{< youtube 1Jvo9I37yAU >}}

[【 **YouTube上观看** 】](https://youtube.com/watch?v=1Jvo9I37yAU)


## 脚本部署工具连接
v2rayN客户端及workers代码都托管在GitHub上，访问时需要科学上网。

### 1、v2rayN下载
V2rayN下载：[https://github.com/2dust/v2rayN/releases](https://github.com/2dust/v2rayN/releases)

### 2、登陆Cloudflare：
官网：[https://www.cloudflare.com](https://www.cloudflare.com)

Cloudflare账户注册小布出过一期Zero Trust团队账户设置视频，其中演示了怎么注册Cloudflare账户，不会注册的朋友可以参考那期视频。

美国地址生成器：[https://haoweichi.com/](https://haoweichi.com/) 

国外地址生成：[https://www.fakenamegenerator.com/](https://www.fakenamegenerator.com/) （需要科学上网）

### 3、部署workers脚本：

Wordkers脚本：

[zizifn worker js](https://github.com/zizifn/edgetunnel/blob/main/src/worker-vless.js)

[3Kmfi6HP worker js](https://github.com/3Kmfi6HP/edtunnel/blob/main/_worker.js)

**【3K大佬已删库】**，有需要的朋友，可以到[ [github.co](https://github.com)m ]上搜“3Kmfi6HP“ ，有网友Fork过的源码。

UUID：[https://1024tools.com/uuid](https://1024tools.com/uuid)

部署中修改两个地方：替换UUID，CF反代IP，反代IP可以使用下面大佬提供的，也可以自己去筛选。如果使用的是[ 3K大佬的 ]worker脚本，替换下伪装域名。

**http** = [80, 8080, 8880, 2052, 2086, 2095, 2082]
**https** = [443, 8443, 2053, 2096, 2087, 2083] 

**大佬提供的CF反代IP：**

> cdn-all.xn--b6gac.eu.org | 
cdn.xn--b6gac.eu.org | 
cdn-b100.xn--b6gac.eu.org | 
edgetunnel.anycast.eu.org | 
cdn.anycast.eu.org | 
> 

**优选域名：**

> icook.hk | 
ip.sb | 
japan.com | 
skk.moe | 
www.visa.com | 
www.visa.co.jp | 
www.visakorea.com | 
www.gco.gov.qa | 
www.csgo.com | 
www.whatismyip.com 
> 

### 4、**套Tls**设置v2rayN（建议套上Tls）

套Tls需要将域名托管到Cloudflare，所以配置之前要想将域名托管设置好

4.1、添加自定义域

4.2、获取vless订阅地址

4.3、v2rayN中添加vless订阅地址

4.4、修改端口为443，传输层安全（Tls）开启

## Workers脚本设置步骤

### 1、创建workers应用

![Untitled](Untitled.webp)

![Untitled](Untitled1.webp)

![Untitled](Untitled2.webp)

### 2、添加workers脚本

2.1、复制github上的workers脚本

![Untitled](Untitled3.webp)

2.2、删除默认脚本并粘贴workers脚本

![Untitled](Untitled4.webp)

### 3、修改UserID和proxyIP

UserID可以通过在线生成UUID得到，也可通过v2rayN创建Vless连接中的生成用户ID得到。

proxyIP可以复制上面大佬提供的反代域名，也可以自己筛选得到

![Untitled](Untitled5.webp)

![Untitled](Untitled6.webp)

### 4、生成应用

4.1、保存并部署部署

![Untitled](Untitled7.webp)

![Untitled](Untitled8.webp)

## 获取未加密的Vless订阅连接

### 1、 打开连接

1.1、点workers.dev (点击此链接前需要科学上网)

![Untitled](Untitled9.webp)

1.2、如果看到下面的代码说明部署成功

![Untitled](Untitled10.webp)

### 2、添加UserID

在连接后面添加UserID（就是workers代码中UserID），注意添加反斜线 “ **/** ”，然后回车

![Untitled](Untitled11.webp)

### 3、生成Vless订阅连接

得到未套Tls的Vless订阅连接

![Untitled](Untitled12.webp)

## 获取Tls加密的Vless订阅连接

操作前需确定域名已经成功托管到Cloudflare上

### 1、添加自定义域

1.1、点击刚刚添加的edge应用

![Untitled](Untitled13.webp)

1.2、点击【查看】，或者点击【触发器】

![Untitled](Untitled14.webp)

1.3、点击【添加自定义域】

![Untitled](Untitled15.webp)

1.4、输入一个二级域名，名称随便起。二级域名要填写完整（**二级域名**.temptemps.top）

然后点击下面的添加自定义域按钮

![Untitled](Untitled16.webp)

1.5、添加自定义域后续等待证书申请完成

![Untitled](Untitled17.webp)

看到有效即证书申请完成

![Untitled](Untitled18.webp)

### 2、 打开连接

2.1、点击刚刚生成的二级域名

![Untitled](Untitled19.webp)

2.2、看到下面代码说明部署成功

![Untitled](Untitled20.webp)

### 3、生成Vless订阅连接

3.1、在域名后添加UserID回车，得到以套Tls的vless订阅连接

![Untitled](Untitled21.webp)

## v2rayN客户端设置

### 1、添加未套Tls的vless连接

1.1、复制未加密vless订阅连接，粘贴到v2rayN客户端

![Untitled](Untitled12.webp)

1.2、打开v3rayN客户端，从剪贴板导入批量URL或者直接按Ctrl+V

![Untitled](Untitled22.webp)

![Untitled](Untitled23.webp)

1.3、双击连接打开配置页面,因为是为加密的vless订阅，不能使用443端口，将443端口改为80,传输层安全（Tls）：设置为空

总共有7个端口可选，可以任选其中一个： [80, 8080, 8880, 2052, 2086, 2095, 2082]

![Untitled](Untitled24.webp)

### 2、添加已套Tls的vless连接

2.1、复制生成的Vless订阅连接

![Untitled](Untitled21.webp)

2.2、粘贴到v2rayN中

![Untitled](Untitled25.webp)

Vless添加后不用修改设置，如果出现不能访问的情况，可将地址修改为优选ip或优选域名

Tls端口总共有6个端口可选，可以任选其中一个： [443, 8443, 2053, 2096, 2087, 2083]

![Untitled](Untitled26.webp)