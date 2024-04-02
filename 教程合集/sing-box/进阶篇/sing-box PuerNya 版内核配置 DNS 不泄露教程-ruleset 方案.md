# [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)配置 DNS 不泄露教程-ruleset 方案
注：
- 1. 此方案适用于 [sing-box](https://github.com/SagerNet/sing-box)，采用 `rule_set` 规则
- 2. 此方案从心理乃至物理层面防止了 DNS 泄露（心理上 DNS 泄露指的是 DNS 模式已经使用了 mix 或 fakeip 的情况下仍觉得 DNS 发生了泄露），配置简单粗暴，兼容性无法保证，请慎用
- 3. 可进入 https://ipleak.net 测试 DNS 是否泄露，“DNS Addresses” 栏目下没有中国国旗，即代表 DNS 没有发生泄露
- 4. 此方案自定义规则参考[DustinWin/ruleset_geodata/ruleset](https://github.com/DustinWin/ruleset_geodata/tree/master#%E4%BA%8C-ruleset-%E8%A7%84%E5%88%99%E9%9B%86%E6%96%87%E4%BB%B6%E8%AF%B4%E6%98%8E)
- 5. 所有步骤完成后，请连接 SSH 执行命令 `$CRASHDIR/start.sh restart` 后生效
# 一、 导入 sing-box PuerNya 版内核
可参考《[ShellCrash 配置-geodata 方案/导入 sing-box PuerNya 版内核](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md#%E4%B8%80-%E5%AF%BC%E5%85%A5-sing-box-puernya-%E7%89%88%E5%86%85%E6%A0%B8)》里的步骤进行操作
# 二、 额外编辑配置文件
在《[生成带有自定义策略组和规则的 sing-box 配置文件直链-ruleset 方案/添加模板和配置文件](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E6%B7%BB%E5%8A%A0%E6%A8%A1%E6%9D%BF%E5%92%8C%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)》编辑 .json 配置文件时，将 route.rules 参数里所有 IP 相关的规则末尾加上 "skip_resolve": true，即修改为：
- 注：若遇兼容性问题（如国内游戏无法登录），可删除所有的 "skip_resolve": true

```
      { "rule_set": [ "telegramip" ], "outbound": "📲 电报消息", "skip_resolve": true },
      { "rule_set": [ "privateip" ],  "outbound": "🔒 私有网络", "skip_resolve": true },
      { "rule_set": [ "cnip" ], "outbound": "🇨🇳 国内 IP", "skip_resolve": true }
```
# 三、 ShellCrash 设置
1. 进入 ShellCrash->7 内核进阶设置->6 配置内置 DNS 服务，将“当前基础 DNS”和“PROXY-DNS”都设置为“null”  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/4ac7a9ce-2c04-4adc-bbaf-8a1281886f5e" width="60%"/>

2. 其它设置可参考《[ShellCrash 配置-ruleset 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md)》
# 四、 `dns` 配置
连接 SSH 后执行 `vi $CRASHDIR/jsons/dns.json`，按一下 Ins 键（Insert 键），粘贴如下内容：
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
      { "domain_regex": [ ".*" ], "query_type": [ "A", "AAAA" ], "server": "dns_fakeip", "rewrite_ttl": 1 },
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
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
