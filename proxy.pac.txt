function FindProxyForURL(url, host) {
    // 1. 直连 Tailscale 虚拟网段 (100.x.x.x)，避免环路
    if (isInNet(host, "100.0.0.0", "255.0.0.0")) {
        return "DIRECT";
    }
    // 2. 直连校园网网段 (10.110.x.x)，避免触发认证
    if (isInNet(host, "10.110.0.0", "255.255.0.0")) {
        return "DIRECT";
    }
    // 3. 直连 Tailscale 相关域名
    if (dnsDomainIs(host, "tailscale.com") || shExpMatch(host, "*.tailscale.com")) {
        return "DIRECT";
    }
    // 4. 直连校园网域名（替换为你的校园网域名，如 *.hxn.edu.cn）
    if (dnsDomainIs(host, "hxn.edu.cn") || shExpMatch(host, "*.hxn.edu.cn")) {
        return "DIRECT";
    }
    // 5. 其他所有流量走 v2rayN 代理（电脑 Tailscale IP:10808 SOCKS5）
    return "SOCKS5 100.97.227.15:10808; SOCKS 100.97.227.15:10808; DIRECT";
}