# [mihomo 内核](https://github.com/MetaCubeX/mihomo)配置 DNS 不泄露教程
注：
- 1. 此方案适用于 [Clash](https://github.com/Dreamacro/clash)，从心理乃至物理层面防止了 DNS 泄露（心理上 DNS 泄露指的是 DNS 模式已经使用了 fake-ip 的情况下仍觉得 DNS 发生了泄露）
- 2. 此方案配置简单粗暴，兼容性无法保证，请慎用
- 3. 可进入 https://ipleak.net 测试 DNS 是否泄露，“DNS Addresses” 栏目下没有中国国旗，即代表 DNS 没有发生泄露
- 4. geodata 方案自定义规则参考[生成带有自定义策略组和规则的 Clash 配置文件直链-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md)
- 5. ruleset 方案自定义规则参考[生成带有自定义策略组和规则的 Clash 配置文件直链-ruleset 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md)

# 一、 geodata 方案
`dns` 配置如下：
```
dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter:
    - '*'
    - '+.lan'
    - '+.local'
  nameserver:
    - 'https://1.1.1.1/dns-query#🪜 代理域名&h3=true'
    - 'https://8.8.8.8/dns-query#🪜 代理域名'
  nameserver-policy:
    'geosite:category-ads-all': rcode://refused
    'geosite:microsoft@cn,apple-cn,google-cn,category-games@cn,cn,private': ['https://223.5.5.5/dns-query#h3=true', https://1.12.12.12/dns-query]
```
# 二、 ruleset 方案
`dns` 配置如下：
```
dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter:
    - '*'
    - '+.lan'
    - '+.local'
  nameserver:
    - 'https://1.1.1.1/dns-query#🪜 代理域名&h3=true'
    - 'https://8.8.8.8/dns-query#🪜 代理域名'
  nameserver-policy:
    'rule-set:ads': rcode://refused
    'rule-set:microsoft-cn,apple-cn,google-cn,games-cn,cn,private': ['https://223.5.5.5/dns-query#h3=true', https://1.12.12.12/dns-query]
```
