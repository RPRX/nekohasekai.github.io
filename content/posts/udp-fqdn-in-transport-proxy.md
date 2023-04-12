---
title: "为什么你不想在透明代理 UDP 时传送域名目标地址到服务器"
date: 2023-04-12T22:09:36+08:00
summary: 大多数服务器的行为会导致错误。
---

大多数服务器的行为会导致错误。

目前没有一个代理协议规定了向服务器发送具有域名的 UDP 包后，服务器应如何处理响应中的地址。实际上，大多数代理程序服务器，如
Shadowsocks 官方实现 和 V2Ray，在响应中只返回 IP 地址。

当代理程序向服务器发送域名时，它并不知道该域名对应的 IP 地址。当服务器在响应中发送 IP
地址时，该地址可能与客户端发起连接的目标地址不匹配。如果客户端验证该地址，该连接将失败。

因此，像 Clash 和 Xray 这样支持 fake-ip 的透明代理客户端会强制在发起 UDP 代理连接之前将域名解析为 IP 地址。因此，实际上你几乎不能这样做。

在 Clash 中：

```go
func handleUDPConn(packet *inbound.PacketAdapter) {
	...

	// local resolve UDP dns
	if !metadata.Resolved() {
		ips, err := resolver.LookupIP(context.Background(), metadata.Host)
		if err != nil || len(ips) == 0 {
			packet.Drop()
			return
		}
		metadata.DstIP = ips[0]
	}
	
	...
}

```

在 Xray 中：

```go
func (d *DefaultDispatcher) getLink(ctx context.Context, network net.Network, sniffing session.SniffingRequest) (*transport.Link, *transport.Link) {
	...

	if addr.Family().IsIP() {
		...
	} else {
		if ip2domain == nil {
			ip2domain = new(sync.Map)
			newError("[fakedns client] create a new map").WriteToLog(session.ExportIDToError(ctx))
		}
		domain := addr.Domain()
		ips, err := d.dns.LookupIP(domain, dns.IPOption{true, true, false})
		if err == nil {
			for _, ip := range ips {
				ip2domain.Store(ip.String(), domain)
			}
			newError("[fakedns client] candidate ip: "+fmt.Sprintf("%v", ips), " for xUDP buffer at ", i).WriteToLog(session.ExportIDToError(ctx))
		} else {
			newError("[fakedns client] failed to look up IP for ", domain, " for xUDP buffer at ", i).Base(err).WriteToLog(session.ExportIDToError(ctx))
		}
	}

	...
}

```

如果您确实想尝试，则请确保您使用 sing-box :)
