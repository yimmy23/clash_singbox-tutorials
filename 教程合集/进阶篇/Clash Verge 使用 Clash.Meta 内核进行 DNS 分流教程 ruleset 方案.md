# [Clash Verge](https://github.com/zzzgydi/clash-verge) 使用 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)进行 DNS 分流教程 ruleset 方案
注：
- 1. 此方案采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
- 2. 只有 **DNS 模式选用 reidir-host（`fake-ip-filter: ['+.*']` 也算 redir-host 模式）** 时才需要进行 DNS 分流
- 3. DNS 分流简单来说就是**指定国内域名走阿里或腾讯 DNS**，主要是这个配置：
```
  nameserver-policy:
    'rule-set:cn': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
```
- 4. 此方案自定义规则参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
---
# 一、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)
可参考《[Clash Verge 配置 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE%20ruleset%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E5%AF%BC%E5%85%A5%E6%88%96%E6%9B%B4%E6%96%B0-clash-meta-%E5%86%85%E6%A0%B8)》里的步骤《二》进行操作
# 二、 额外编辑配置文件
1. 在《[生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20ruleset%20%E6%96%B9%E6%A1%88.md)》编辑 .yaml 配置文件时，建议在 `tun` 参数中加上 `strict-route: true`，即修改为：
```
tun:
  enable: true
  stack: system
  dns-hijack:
    - 'any:53'
  auto-route: true
  auto-detect-interface: true
  # 严格路由，可防止地址泄漏，启用后你的设备将无法被其他设备访问
  strict-route: true
```
2. 如果使用的是白名单模式，建议将 `rules` 里的规则 `- GEOIP,cn,🇨🇳 国内 IP` 后面加上 `no-resolve`，即修改为：
```
  - GEOIP,cn,🇨🇳 国内 IP,no-resolve
```
# 三、 编辑自定义配置
## 1. DNS 模式为 fake-ip
- 注：该模式不需要进行 DNS 分流，推荐导入我生成的 fake-ip-user.yaml（集成 [fake-ip 地址过滤列表](https://github.com/juewuy/ShellClash/blob/master/public/fake_ip_filter.list)，提高了兼容性）

① 进入 Clash Verge->配置，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”  
② 进入文件夹 *%USERPROFILE%\\.config\clash-verge\profiles*，找到与第 1 步新建的 Merge 文件相对应的 .yaml 文件，复制其文件名并替换下面命令中的{文件名}  
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %USERPROFILE%\.config\clash-verge\profiles\{Merge 文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tutorials@main/fake-ip-config/fake-ip-user.yaml
```
## 2. DNS 模式为 redir-host
① 白名单模式（没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）  
进入 Clash Verge->配置，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”  
再次右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  use-hosts: true
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  default-nameserver:
    - https://223.5.5.5/dns-query
    - https://1.12.12.12/dns-query
  nameserver:
    - tls://dns.google
    - https://cloudflare-dns.com/dns-query
  proxy-server-nameserver:
    - https://dns.alidns.com/dns-query
    - https://doh.pub/dns-query
  nameserver-policy:
    'rule-set:microsoft-cn,apple-cn,google-cn,games-cn': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
    'rule-set:cn,lan': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
```
② 黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）  
进入 Clash Verge->配置，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”  
再次右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  use-hosts: true
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  default-nameserver:
    - https://223.5.5.5/dns-query
    - https://1.12.12.12/dns-query
  nameserver:
    - https://dns.alidns.com/dns-query
    - https://doh.pub/dns-query
  nameserver-policy:
    'rule-set:proxy': [tls://dns.google, https://cloudflare-dns.com/dns-query]
```
