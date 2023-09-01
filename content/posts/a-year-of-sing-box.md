---
title: "一年后的 sing-box"
date: 2023-08-27T12:02:16+08:00
summary: :)
---

距离 sing-box 1.0 发布将满一年，star 数量也来到了 4k，我想这是一个值得庆祝的日子。

首先我要感谢对社区作出了很大贡献的开发者：

* wwqgtxx
* dyhkwong
* iKirby
* StudentMain

和其他贡献者：

* xiaokangwang
* XYenon
* zakuwaki

以及大量 typo 修复和不痛不痒更改请求的发起者无名朋友们。

在这一年里，sing-box 作出了大量改进，包括：

* 更好的 DNS 支持，包括所有 DNS transport、DNS 路由、`reverse_mapping` 和更好的 fake-ip 支持 （包括 IPv6、0
  延迟 `store_fakeip`、与路由集成等独有功能）。
* sing-tun 现在不仅是唯一一个开源的带有自动路由功能与 system stack 的 L3 透明代理程序库， 而且是唯一一个支持包括 iOS
  在内的所有主流平台的。
* 最先与最好的 WireGuard 集成，包括 gVisor 非特权模式、 tun 特权模式 与自定义 reserved bytes
  支持，此后 Golang 同类项目不管是否使用 GPL 授权，均有复制来自于 sing-box 的相关代码。
* 高性能 shadow-tls v1-v3实现
* 高性能 TUIC 实现、包括 `udp_over_stream` 支持
* 新的 `UDP over TCP` 与 `Multiplex` 协议

同时，我为 `sing-box` 编写了全新的 `Android`、`iOS`、 `macOS` 与 `Apple tvOS` 客户端，这些客户端不仅允许您管理并运行本地、iCloud
或远程 sing-box 配置，还提供了大量平台特定的功能实现与改进，包括：

* Apple 平台唯一全系统支持的免费和开源代理项目
* 在 Apple 平台与 Android 提供休眠支持，在设备闲置时暂停包括 `URLTest` 与 `WireGuard` 定时器在内的定时任务
* 在 Android 平台提供首创的分应用代理与 `中国应用` 检测功能，并且支持在应用安装或更新时自动检测并作为分应用项选中
* 提供状态面板， `URLTest`、`Selector` 等出站组的状态显示与切换功能。

再次感谢为社区作出贡献的朋友们。