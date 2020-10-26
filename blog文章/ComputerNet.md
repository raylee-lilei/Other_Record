---
title: ComputerNet
author: raylee
avatar: 'https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.4/img/custom/logo_1.png'
authorLink: 
authorAbout: industry pays
authorDesc: industry pays
categories: technology
comments: true
date: 2020-04-03 16:50:18
tags:
keywords: Computer Net
description:
photos: https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/os.jpg
---

# 计算机网路概述

## 局域网(LAN)
● 同一网络下，不同计算机使用的是局域网，覆盖范围小，自己花钱买设备，贷款固定，自己维护
## 广域网（WAN）
● 链接不同地区的局域网，形成一个大范围的网络

## 数据通讯过程
客户端到服务器端发生了什么？


图片1

每个计算机有自己的IP和mac地址（计算机的物理地址），子网掩码255代表网络部分，0代表主机部分
当我们在浏览器输入URL时，DNS把我们的域名解析成相应URL 网页存放的IP地址，此时访问计算机就知道被访问的IP地址。

数据包：包括数据和本机ip 和目标ip 

data|本机IP|目标IP
:-|:-|:-
数据|12.0.0.1|17.0.0.2

数据帧：数据包加上本机和目标服务器的mac地址

data|本机IP|目标IP|本机mac|目标mac
:-|-:|-:|-:|-:|
数据|12.0.0.1|17.0.0.2|mac本机|mac目标

往下传播时，更新数据帧mac 地址，去找到最终存放网页数据IP计算机，A—F—D—H。服务器收到客户端的请求之后，会反馈给客户端，首先，服务器把网页切割成小的数据包，一个一个放入缓存区里面，传一个给客户端之后，客户端收到数据，再反馈服务器端传下一个数据包（若果客户端不给反馈，服务器端会重新再发一个数据包 ），更新缓存区，知道数据接收完为止。

## 数据通讯的OSI模型
**●应用层：**所有能产生网络流量的应用程序
**●表示层：**在传输之前是否进行加密或者压缩 （二进制、ASCLL）
**●会话层：**查看传输过程中的木马 netstat -n, 是否存在其他病毒干扰传输
**●传输层：**可靠传输，流量控制，不可靠传输（直接传输，客户端直接到达DNS传输）
**●网络层：**负责选择最佳路径，规划IP地址
**●数据链路层：**帧的开始和结束，透明传输，差错校验
**●物理层：**接口标准，电器压标准，如何在物理链路上传输更快
