# [Clash Verge](https://github.com/clash-verge-rev/clash-verge-rev) 使用 [mihomo 内核](https://github.com/MetaCubeX/mihomo)进行 DNS 分流教程-geodata 方案
注：
- 1. 此方案适用于 [Clash](https://github.com/Dreamacro/clash)，采用 GEOSITE 和 GEOIP 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb）[路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
- 2. 只有 **DNS 模式选用 `reidir-host`（`fake-ip-filter: ['+.*']` 也算 redir-host 模式）** 时才需要进行 DNS 分流
- 3. DNS 分流简单来说就是**指定国内域名走国内 DNS 解析，国外域名走国外 DNS 解析**
- 4. 此方案自定义规则参考 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat)
---
# 一、 导入 mihomo 内核和路由规则文件
可参考《[Clash Verge 配置-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md)》里的步骤《一、二》进行操作
# 二、 编辑自定义配置
## 1. DNS 模式为 `fake-ip`
- 注：该模式不需要进行 DNS 分流，推荐导入我生成的 fakeip-user.yaml（集成 [fake-ip 地址过滤列表](https://github.com/juewuy/ShellClash/blob/dev/public/fake_ip_filter.list)，提高了兼容性）

① 进入 Clash Verge -> 订阅，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”  
② 进入文件夹 *%APPDATA%\io.github.clash-verge-rev.clash-verge-rev\profiles*，找到与上一步新建的 Merge 文件相对应的 .yaml 文件，复制其文件名并替换下面命令中的 `{Merge 文件名}`  
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im Clash-Verge*
taskkill /f /t /im clash-meta*
curl -o "%APPDATA%\io.github.clash-verge-rev.clash-verge-rev\profiles\{Merge 文件名}.yaml" -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tutorials@clash/fakeip-user.yaml
```
③ 再次进入 Clash Verge -> 订阅，右击新建的 Merge 文件，点击“启用”
## 2. DNS 模式为 `redir-host`
① 进入 Clash Verge -> 订阅，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”  
② 右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
sniffer:
  enable: true
  parse-pure-ip: true
  sniff: {HTTP: {ports: [80, 8080-8880]}, TLS: {ports: [443, 8443]}, QUIC: {ports: [443, 8443]}}
  skip-domain: ['Mijia Cloud']

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  nameserver:
    - https://223.5.5.5/dns-query
    - https://1.12.12.12/dns-query
  nameserver-policy:
    'geosite:category-ads-all': rcode://refused
    'geosite:microsoft@cn,apple-cn,google-cn,category-games@cn,cn,private': [https://223.5.5.5/dns-query, https://1.12.12.12/dns-query]
    'geosite:geolocation-!cn': ['https://1.1.1.1/dns-query#🪜 代理域名', 'https://8.8.8.8/dns-query#🪜 代理域名']
```
③ 再次右击新建的 Merge 文件，点击“启用”
# 三、 客户端设置
进入 Clash Verge -> 系统设置 -> Tun 模式，点击右边的螺帽图标，启用“严格路由”，然后点击“保存”
