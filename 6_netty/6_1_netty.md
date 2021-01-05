---
title: "[Netty] 1.Netty基础"
date: 2021-01-05T10:36:27+08:00
lastmod: 2021-01-05T10:36:27+08:00
keywords: ['netty']
description: ""
tags: ['java','netty','io']
categories: ['java']
author: ""
---
# Netty基础

## 1.Netty面试
1. 为什么用Netty？
    ```shell
    a. Netty是一个基于JDK NIO的Client/Server架构的框架。可以快速进行网络开发;
    b. 相比JDK NIO，极大简化TCP、UDP套接字服务器网络变成，并且性能、安全性出色;
    c. 支持多种协议：FTP、SMTP、HTTP以及各种二进制和基于文本的传统协议;
    d. 统一的API、支持多种传输类型(阻塞、非阻塞I/O);
    e. 简单且强大的线程模型;
    f. 自带编解码器解决粘包、拆包问题;
    g. 自带各种协议栈;
    h. 安全性不错，支持完整的SSL/TLS及StartTLS协议;
    i. 相比JDK NIO，API具有更高吞吐量、更低延迟、更低资源消耗和更少的内存复制;
    j. 成熟项目很多：Dubbo、RocketMQ。
    ```

2. Netty应用场景
    ```shell
    a. RPC框架的网络通信工具
    b. 自实现HTTP服务器
    c. 实现即时通信系统
    d. 实现消息推送系统
    ```
3. Netty核心组件
   + Channel
     
     对网络操作的抽象类。除I/O基本操作之外，支持`bind()`,`connect()`,`read()`,`write()`操作。

   + EventLoop

     EventLoop是Netty的核心抽象，用于处理连接在生命周期中所发生的事件。
     主要作用：负责监听网络事件并调用事件处理器进行相关的I/O操作。

     **`EventLoop`** 负责处理注册到其上的 **`Channel`** 处理I/O操作，两者配合参与I/O操作。

   + ChannelFuture

     Netty是`异步非阻塞`，所有的I/O操作都是异步。所以不可以立即得到操作结果。但是

     （1）通过 **`ChannelFuture`** 接口的 **`addListener()`** 注册一个 **`ChannelFutureListener`** 对象，操作执行成功或失败后，监听会自动触发返回结果；

     （2）也可以通过 **`ChannelFuture`** 的 **`channel()`** 获取关联的 **`Channel`** 对象；

     （3) 也可以通过 **`ChannelFuture`** 的 **`sync()`** 进行让异步的操作变成同步。
   + ChannelHandler与ChannelPipeline

    
4. Netty线程模型
5. EventLoopGroup是什么？与EventLoop关系。
6. Netty中TCP粘包、拆包
7. Netty长连接、心跳机制
8. Netty零拷贝