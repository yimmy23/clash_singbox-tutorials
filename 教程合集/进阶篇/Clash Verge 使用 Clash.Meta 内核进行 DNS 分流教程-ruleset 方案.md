# [Clash Verge](https://github.com/MetaCubeX/clash-verge) 使用 [Clash.Meta 内核](https://github.com/MetaCubeX/mihomo)进行 DNS 分流教程-ruleset 方案
注：
- 1. 此方案采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
- 2. 只有 **DNS 模式选用 `reidir-host`（`fake-ip-filter: ['+.*']` 也算 redir-host 模式）** 时才需要进行 DNS 分流
- 3. DNS 分流简单来说就是**指定国内域名走腾讯或阿里 DNS**，主要是这个配置：
```
  nameserver-policy:
    'rule-set:cn': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
```
- 4. 此方案自定义规则参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
---
# 一、 导入 Clash.Meta 内核
可参考《[Clash Verge 配置-ruleset 方案/导入或更新 Clash Meta 内核](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E5%AF%BC%E5%85%A5%E6%88%96%E6%9B%B4%E6%96%B0-clash-meta-%E5%86%85%E6%A0%B8)》里的步骤《二》进行操作
# 二、 额外编辑配置文件
1. 在《[生成带有自定义策略组和规则的 yaml 配置文件直链-ruleset 方案/编辑 .yaml 文件](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20yaml%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md#%E4%B8%89-%E7%BC%96%E8%BE%91-yaml-%E6%96%87%E4%BB%B6)》编辑 .yaml 配置文件时，将 `rules` 参数里的所有 IP 相关规则末尾加上 `no-resolve`，即修改为：
- 注：若遇兼容性问题（如国内游戏无法登录），可删除 `- GEOIP,cn` 后面的 `no-resolve`
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
  dns-hijack: ['any:53']
  auto-route: true
  auto-detect-interface: true
  # 严格路由，可防止地址泄漏，启用后你的设备将无法被其他设备访问
  strict-route: true
```
# 三、 编辑自定义配置
## 1. DNS 模式为 `fake-ip`
注：
- 1. 该模式其实不需要进行 DNS 分流，推荐导入我生成的 fake-ip-user.yaml（集成 [fake-ip 地址过滤列表](https://github.com/juewuy/ShellClash/blob/master/public/fake_ip_filter.list)，提高了兼容性）
- 2. 策略组内必须有 `🪜 代理域名`

① 进入 Clash Verge->配置，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”  
② 进入文件夹 *%USERPROFILE%\\.config\clash-verge\profiles*，找到与第 1 步新建的 Merge 文件相对应的 .yaml 文件，复制其文件名并替换下面命令中的{文件名}  
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im Clash-Verge*
taskkill /f /t /im mihomo*
curl -o %USERPROFILE%\.config\clash-verge\profiles\{Merge 文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tutorials@main/fake-ip-config/ruleset-user.yaml
```
## 2. DNS 模式为 redir-host
进入 Clash Verge->配置，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”  
再次右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
sniffer:
  enable: true
  sniff: {HTTP: {ports: [80, 8080-8880], override-destination: true}, TLS: {ports: [443, 8443]}, QUIC: {ports: [443, 8443]}}
  skip-domain: ['Mijia Cloud']

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  default-nameserver:
    - https://1.12.12.12/dns-query
    - https://223.5.5.5/dns-query
  nameserver:
    # 策略组内必须有`🪜 代理域名`
    - 'https://dns.google/dns-query#🪜 代理域名'
    - 'https://cloudflare-dns.com/dns-query#🪜 代理域名'
  proxy-server-nameserver:
    - https://doh.pub/dns-query
  nameserver-policy:
    'rule-set:microsoft-cn,apple-cn,google-cn,games-cn': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
    'rule-set:cn,private': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
```
