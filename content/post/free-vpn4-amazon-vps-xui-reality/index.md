---
title: "[免费科学上网-4] 使用亚马逊免费VPS，打造自己的免费专属机场"
date: 2023-11-14
draft: false
description: "通过亚马逊免费VPS+Reality协议+Vision流控技术，无需域名和证书，打造自己的专属机场"
keywords: ["AWS VPS 新手申请", "亚马逊云 12 个月免费额度", "EC2 弹性实例创建指南", "国外免费云主机获取", "企业级 VPS 建站首选"]
url: "aws-vps"
image: free-vpn4-amazon-vps-xui-reality.webp
categories:
    - Internet
tags:
    - 网络
---

<aside>
😀 上一篇我们通过IP优选将IP限制在了固定的区域，但IP还会在小范围跳动。今天我们通过申请亚马逊免费VPS，创建拥有独立IP、范围固定，真正属于自己的机场节点。平台上有许多自建节点教程，但对于许多没有网络基础的小伙伴来说难度还是不小的。今天小布就分享一种节点配置步骤少，操作相对容易，既符合现在的网络安全要求，又能满足使用的节点搭建方法。

</aside>

{{< youtube lPvOC2XlWe8 >}}

[【 **YouTube上观看** 】](https://youtube.com/watch?v=lPvOC2XlWe)

# 工具链接：

## 亚马逊免费VPS申请链接

[https://aws.amazon.com/cn/free](https://aws.amazon.com/cn/free)

## SSH工具：FinalShell
* Windows：[https://www.hostbuf.com/t/988.html](https://www.hostbuf.com/t/988.html)
* macOS：[http://www.hostbuf.com/downloads/finalshell_install.pkg](http://www.hostbuf.com/downloads/finalshell_install.pkg)
* Linux：[http://www.hostbuf.com/t/1059.html](http://www.hostbuf.com/t/1059.html)

**X-UI：**
* bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/956bf85bbac978d56c0e319c5fac2d6db7df9564/install.sh) 0.3.4.4
**or**
* bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh)

## 代理客户端软件

* Windows（v2rayN-免费）：[https://github.com/2dust/v2rayN/releases](https://github.com/2dust/v2rayN/releases)
* Android（v2rayNG-免费）：[https://github.com/2dust/v2rayNG/releases](https://github.com/2dust/v2rayNG/releases)
* IOS（shadowrocket-收费）：[https://apps.apple.com/us/app/shadowrocket/id932747118](https://apps.apple.com/us/app/shadowrocket/id932747118)

## IP检测工具

* 1、[https://ip.gs/](https://ip.gs/)
* 2、[https://ipdata.co/](https://ipdata.co/)

## IP纯净度检测工具

[https://scamalytics.com/](https://scamalytics.com/) 

## 查找适合Reality的目标网站

* **ASN查询工具：**[https://tools.ipip.net/as.php](https://tools.ipip.net/as.php)
* **目标网站查询工具：**[https://fofa.info](https://fofa.info)

**查询命令：**

asn=="16509" && country=="SG" && port=="443" && cert!="Let's Encrypt" && cert.issuer!="ZeroSSL" && status_code="200"

**asn：**（自治域号码）

**country=="SG"** （国家地区两字码，新加坡：SG，美国：US，日本：JP，韩国：KR，香港：HK，英国：GB，泰国：TH，台湾：TW）两字码查询：[https://baike.baidu.com/item/世界各国和地区名称代码/6560023](https://baike.baidu.com/item/%E4%B8%96%E7%95%8C%E5%90%84%E5%9B%BD%E5%92%8C%E5%9C%B0%E5%8C%BA%E5%90%8D%E7%A7%B0%E4%BB%A3%E7%A0%81/6560023)

**port=="443"** （端口）

**cert!="Let's Encrypt"** （不是Let's Encrypt类型的证书）

**cert.issuer!="ZeroSSL"**（证书颁发者不是ZeroSSL）

**status_code="200"**（HTTP 响应状态码，200的意思是Http请求成功）

Let's Encrypt与ZeroSSL都是免费证书，有效期都是90天

## 端口否被封检测

[https://ping.pe/](https://ping.pe/)  

格式：**https://ping.pe/123.123.123.123:20001**

123.123.123.123 （vps-ip）

20001（vps端口）

###