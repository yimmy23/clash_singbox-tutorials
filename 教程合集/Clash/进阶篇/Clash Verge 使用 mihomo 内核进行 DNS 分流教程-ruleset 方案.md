# [Clash Verge](https://github.com/clash-verge-rev/clash-verge-rev) 使用 [mihomo 内核](https://github.com/MetaCubeX/mihomo)进行 DNS 分流教程-ruleset 方案
注：
- 1. 此方案此方案适用于 [Clash](https://github.com/Dreamacro/clash)，采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
- 2. 只有 **DNS 模式选用 `reidir-host`（`fake-ip-filter: ['+.*']` 也算 redir-host 模式）** 时才需要进行 DNS 分流
- 3. DNS 分流简单来说就是**指定国内域名走国内 DNS 解析，其余域名走国外 DNS 解析**
- 4. 此方案自定义规则参考 [DustinWin/ruleset_geodata/ruleset](https://github.com/DustinWin/ruleset_geodata/tree/master#%E4%BA%8C-ruleset-%E8%A7%84%E5%88%99%E9%9B%86%E6%96%87%E4%BB%B6%E8%AF%B4%E6%98%8E)
---
# 一、 导入 mihomo 内核
可参考《[Clash Verge 配置-ruleset 方案/导入或更新 mihomo 内核](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md#%E4%B8%80-%E5%AF%BC%E5%85%A5%E6%88%96%E6%9B%B4%E6%96%B0-mihomo-%E5%86%85%E6%A0%B8)》进行操作
# 二、 额外编辑配置文件
1. 在《[生成带有自定义策略组和规则的 Clash 配置文件直链-ruleset 方案/添加模板](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E6%B7%BB%E5%8A%A0%E6%A8%A1%E6%9D%BF)》编辑 .yaml 配置文件时，将 `rules` 参数里的所有 IP 相关规则末尾加上 `no-resolve`，即修改为：
- 注：若遇兼容性问题（如国内游戏无法登录），可删除所有的 `no-resolve`
```
  - RULE-SET,telegramip,📲 电报消息,no-resolve
  - RULE-SET,privateip,🔒 私有网络,no-resolve
  - RULE-SET,cnip,🇨🇳 国内 IP,no-resolve
```
2. 在 `tun` 参数中加上 `strict-route: true`，即修改为：
```
tun:
  enable: true
  stack: system
  dns-hijack: [any:53]
  auto-route: true
  auto-detect-interface: true
  # 严格路由，可防止地址泄漏，启用后你的设备将无法被其他设备访问
  strict-route: true
```
# 三、 编辑自定义配置
## 1. DNS 模式为 `fake-ip`
- 注：该模式不需要进行 DNS 分流，推荐导入我生成的 fakeip-user.yaml（集成 [fake-ip 地址过滤列表](https://github.com/juewuy/ShellClash/blob/dev/public/fake_ip_filter.list)，提高了兼容性）

① 进入 Clash Verge->订阅，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”  
② 进入文件夹 *%APPDATA%\io.github.clash-verge-rev.clash-verge-rev\profiles*，找到与上一步新建的 Merge 文件相对应的 .yaml 文件，复制其文件名并替换下面命令中的 `{Merge 文件名}`  
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im Clash-Verge*
taskkill /f /t /im clash-meta*
curl -o %APPDATA%\io.github.clash-verge-rev.clash-verge-rev\profiles\{Merge 文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tutorials@clash/fakeip-user.yaml
```
③ 再次进入 Clash Verge->订阅，右击新建的 Merge 文件，点击“启用”
## 2. DNS 模式为 `redir-host`
- 注：策略组内**必须有 🪜 代理域名，其选中的机场节点不能支持 IPv6**（可全局代理后进入 [IPv6 测试](https://www.test-ipv6.com)网站来测试机场某节点是否支持 IPv6）

① 进入 Clash Verge->订阅，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”  
② 右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
sniffer:
  enable: true
  parse-pure-ip: true
  sniff: {HTTP: {ports: [80, 8080-8880], override-destination: true}, TLS: {ports: [443, 8443]}, QUIC: {ports: [443, 8443]}}
  skip-domain: [Mijia Cloud]

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  nameserver:
    - 'https://223.5.5.5/dns-query#h3=true'
    - https://1.12.12.12/dns-query
  nameserver-policy:
    'rule-set:ads': rcode://refused
    'rule-set:microsoft-cn,apple-cn,google-cn,games-cn,cn,private': ['https://223.5.5.5/dns-query#h3=true', https://1.12.12.12/dns-query]
    # 策略组内必须有 `🪜 代理域名` 且不支持 IPv6
    'rule-set:proxy': ['https://1.1.1.1/dns-query#🪜 代理域名&h3=true', 'https://8.8.8.8/dns-query#🪜 代理域名']
```
③ 再次右击新建的 Merge 文件，点击“启用”
# 四、 客户端设置
进入 Clash Verge->系统设置->Tun 模式，点击右边的螺帽图标，启用“严格路由”，然后点击“保存”
