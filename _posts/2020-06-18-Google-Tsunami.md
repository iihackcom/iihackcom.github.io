---
layer: article
title: 后起还未秀之GOogle Tsunami
tags: tools pentest
---
## 更新

<https://opensource.googleblog.com/2021/02/updates-on-tsunami-security-scanning-engine.html>



## 2020-6-18 发布

Google Open Source发布了网络安全扫描引擎[Tsunami](https://github.com/google/tsunami-security-scanner)，插件库[Plugins for Tsunami Security Scanner](https://github.com/google/tsunami-security-scanner-plugins)

扫描步骤：

1. 侦察

   检测开放端口->指纹识别协议、服务和其他软件。

   使用nmap等实现。

2. 漏洞验证

   根据侦察结果匹配漏洞验证插件。

   使用ncrack检测弱密码等。

<https://opensource.googleblog.com/2020/06/tsunami-extensible-network-scanning.html>