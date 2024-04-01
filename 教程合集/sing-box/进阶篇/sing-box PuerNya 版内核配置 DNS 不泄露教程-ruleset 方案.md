# [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)配置 DNS 不泄露教程-ruleset 方案
注：
- 1. 此方案适用于 [sing-box](https://github.com/SagerNet/sing-box)，采用 `rule_set` 规则
- 2. 此方案从心理乃至物理层面防止了 DNS 泄露（心理上 DNS 泄露指的是 DNS 模式已经使用了 mix 或 fakeip 的情况下仍觉得 DNS 发生了泄露），配置简单粗暴，兼容性无法保证，请慎用
- 3. 可进入 https://ipleak.net 测试 DNS 是否泄露，“DNS Addresses” 栏目下没有中国国旗，即代表 DNS 没有发生泄露
- 4. ruleset 方案自定义规则参考[生成带有自定义出站和规则的 sing-box 配置文件直链-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md)

`dns` 配置如下：
```
{
  "dns": {
    "servers": [
      { "tag": "dns_block", "address": "rcode://refused" },
      { "tag": "dns_alidns", "address": "h3://223.5.5.5/dns-query", "detour": "DIRECT" },
      { "tag": "dns_dnspod", "address": "https://1.12.12.12/dns-query", "detour": "DIRECT" },
      { "tag": "dns_cloudflare", "address": "h3://1.1.1.1/dns-query" },
      { "tag": "dns_google", "address": "https://8.8.8.8/dns-query" },
      { "tag": "dns_fakeip", "address": "fakeip" }
    ],
    "rules": [
      { "outbound": "any", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Direct", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Global", "server": "dns_fakeip", "rewrite_ttl": 1 },
      { "rule_set": [ "ads" ], "server": "dns_block" },
      { "rule_set": [ "microsoft-cn", "apple-cn", "google-cn", "games-cn", "cn", "private" ], "query_type": [ "A", "AAAA" ], "server": [ "dns_alidns", "dns_dnspod" ] },
      { "rule_set": [ "geolocation-!cn" ], "query_type": [ "A", "AAAA" ], "server": "dns_fakeip", "rewrite_ttl": 1 },
      { "fallback_rules": [ { "rule_set": [ "cnip" ], "invert": true } ], "server": [ "dns_cloudflare", "dns_google" ] }
    ],
    "final": [ "dns_cloudflare", "dns_google" ],
    "strategy": "prefer_ipv4",
    "independent_cache": true,
    "reverse_mapping": true,
    "fakeip": { "enabled": true, "inet4_range": "198.18.0.0/15", "inet6_range": "fc00::/18" }
  }
}
```
