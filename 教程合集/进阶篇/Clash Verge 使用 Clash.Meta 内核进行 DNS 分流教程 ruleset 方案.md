# [Clash Verge](https://github.com/zzzgydi/clash-verge) 使用 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)进行 DNS 分流教程 ruleset 方案
注：
- 1. 此方案采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
- 2. DNS 分流简单来说就是**指定国内域名走阿里或腾讯 DNS**，主要是这个配置：
```
nameserver-policy:
  'rule-set:cn': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
```
- 3. 此方案自定义规则参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
---
# 一、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)和路由规则文件
可参考《[Clash Verge 配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE.md)》里的步骤《二、三》进行操作
# 二、 编辑自定义配置
## 1. DNS 模式为 fake-ip 
该模式不需要使用 DNS 分流，仅配置 `default-nameserver` 和 `nameserver` 这两个参数即可  
1. 进入 Clash Verge->配置，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”
2. 进入文件夹 *%USERPROFILE%\\.config\clash-verge\profiles*，找到与第 1 步新建的 Merge 文件相对应的 .yaml 文件，复制其文件名并替换下面命令中的{文件名}  
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %USERPROFILE%\.config\clash-verge\profiles\{Merge 文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tutorials@main/dns-bypass/fake-ip-user.yaml
```
## 2. DNS 模式为 redir-host
该模式需要使用 DNS 分流  
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
    - https://1.12.12.12/dns-query
    - https://223.5.5.5/dns-query
  nameserver:
    - tls://dns.google
    - https://dns.cloudflare.com/dns-query
    - https://doh.opendns.com/dns-query
  proxy-server-nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  nameserver-policy:
    'rule-set:networktest,microsoft-cn,apple-cn,google-cn,games-cn': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
    'geosite:direct,lan': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
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
    - https://1.12.12.12/dns-query
    - https://223.5.5.5/dns-query
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  nameserver-policy:
    'rule-set:proxy': [tls://dns.google, https://dns.cloudflare.com/dns-query, https://doh.opendns.com/dns-query]
```
