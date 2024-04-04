# [ShellCrash](https://github.com/juewuy/ShellCrash) 使用 [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)进行 DNS 分流教程-geodata 方案
注：
- 1. 此方案适用于 [sing-box](https://github.com/SagerNet/sing-box)，采用 geosite 和 geoip 规则搭配 geosite.db 和 geoip.db [路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
- 2. 搭配 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 并将 AdGuardHome 作为上游时不要使用该方法
- 3. DNS 分流简单来说就是**指定国内域名走国内 DNS 解析，其它域名包括国外域名走 fakeip**
- 5. 此方案自定义规则参考 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat)
- 6. 所有步骤完成后，请连接 SSH 执行命令 `$CRASHDIR/start.sh restart` 后生效
---
# 一、 导入 sing-box PuerNya 版内核
可参考《[ShellCrash 配置-geodata 方案/导入 sing-box PuerNya 版内核](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md#%E4%B8%80-%E5%AF%BC%E5%85%A5-sing-box-puernya-%E7%89%88%E5%86%85%E6%A0%B8)》里的步骤进行操作
# 二、 ShellCrash 设置
1. 进入 ShellCrash->7 内核进阶设置->6 配置内置 DNS 服务，将“当前基础 DNS”和“PROXY-DNS”都设置为“null”  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/4ac7a9ce-2c04-4adc-bbaf-8a1281886f5e" width="60%"/>

2. 其它设置可参考《[ShellCrash 配置-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md)》
# 三、 导入 dns.json 文件
注：
- 1. `dns.rules` 中的 `"geosite": [ "private" ]` 已添加[常用 fake-ip 地址过滤列表](https://github.com/juewuy/ShellCrash/blob/dev/public/fake_ip_filter.list)里的所有 AdGuardHome 相关域名，防止作为下游时检查更新和下载“DNS 黑名单”失败
- 2. `dns.rules` 中的 `"geosite": [ "private" ]` 已添加 [TrackersList](https://github.com/XIU2/TrackersListCollection/blob/master/all.txt)，防止 [BT 下载](https://github.com/c0re100/qBittorrent-Enhanced-Edition)无法连接 TrackersList UDP 协议

连接 SSH 后执行 `vi $CRASHDIR/jsons/dns.json`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
{
  "dns": {
    "servers": [
      { "tag": "dns_alidns", "address": "h3://223.5.5.5/dns-query", "detour": "DIRECT" },
      { "tag": "dns_dnspod", "address": "https://1.12.12.12/dns-query", "detour": "DIRECT" },
      { "tag": "dns_fakeip", "address": "fakeip" }
    ],
    "rules": [
      { "outbound": "any", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Direct", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Global", "server": "dns_fakeip", "rewrite_ttl": 1 },
      { "domain_keyword": [ "." ], "invert": true, "query_type": [ "A", "AAAA" ], "server": [ "dns_alidns", "dns_dnspod" ] },
      { "protocol": [ "stun" ], "query_type": [ "A", "AAAA" ], "server": [ "dns_alidns", "dns_dnspod" ] },
      { "geosite": [ "microsoft-cn", "apple-cn", "google-cn", "games-cn", "cn", "private" ], "query_type": [ "A", "AAAA" ], "server": [ "dns_alidns", "dns_dnspod" ] },
      { "query_type": [ "A", "AAAA" ], "server": "dns_fakeip", "rewrite_ttl": 1 }
    ],
    "final": [ "dns_alidns", "dns_dnspod" ],
    "strategy": "prefer_ipv4",
    "independent_cache": true,
    "reverse_mapping": true,
    "fakeip": { "enabled": true, "inet4_range": "198.18.0.0/15", "inet6_range": "fc00::/18" }
  }
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
