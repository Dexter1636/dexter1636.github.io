title = "从JDK8升级到JDK11后的SNI问题"
description = ""
math = false
type = ["posts","post"]
tags = [
    "development",
    "troubleshooting",
    "jdk11",
    "network"
]
date = 2023-12-09
categories = [
    "Development",
    "Troubleshooting"
]
series = []
[ author ]
  name = "Dexter"

+++



### TL;DR

一个项目在从JDK8升级到JDK11后，通过代理服务器调用谷歌服务时报错 “无法找到有效证书路径”：

```
javax.net.ssl.SSLHandshakeException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

抓包发现SSL握手时谷歌服务器返回了一张无效证书，并在证书中提示`No SNI provided; please fix your client.`





### 问题

#### 问题描述



#### 问题复现





### 背景知识

#### 什么是SNI

服务器名称指示（SNI，Server Name Indication）是TLS的一个扩展协议。在该协议下，客户端在握手开始时通过Client Hello报文告诉服务端它要连接的主机名称。

很多时候服务端会透过同一个IP地址提供多个HTTPS服务，不同的HTTPS服务通常又对应着不同的证书。在此情况下，服务端就可以通过客户端传递的SNI信息选择对应的正确证书返回给客户端。如果客户端握手时未携带SNI，则很可能收到默认证书，导致证书校验失败。



### 根因剖析





### 解决方案





### 总结





### Reference

[Server Name Indication - Wikipedia](https://en.wikipedia.org/wiki/Server_Name_Indication)

