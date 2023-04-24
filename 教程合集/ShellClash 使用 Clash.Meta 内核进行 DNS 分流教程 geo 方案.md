# 解释：
此方案采用 GEOSITE 和 GEOIP 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） 路由规则文件  
DNS 分流简单来说就是**指定国内域名走阿里或腾讯 DNS**，主要是这个配置：
```
nameserver-policy:
  'geosite:cn': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
```
---
# 一、 方法
- 注：搭配 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 时不要使用该方法

## 1. 安装并升级内核
安装 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)并升级到 v1.14.2+版本，方法请看[安装 Clash.Meta 内核](https://github.com/DustinWin/Clash-Tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellClash%20%E5%92%8C%20AdGuardHome%20%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.md#%E4%BA%8C-%E5%AE%89%E8%A3%85-clashmeta-%E5%86%85%E6%A0%B8)
## 2. 导入 user.yaml 文件
将 user.yaml 文件移动到 ShellClash 安装目录（如 */data/clash*）  
或者使用快速导入方法（使用此方法可略过第“三”步）：  
① 使用 fake-ip 模式，连接 SSH 后执行如下命令：
```
curl -o $clashdir/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/DNS-Bypass/geo-mode/fake-ip-mode/user.yaml && $clashdir/start.sh restart
```
② 使用 redir-host 模式，连接 SSH 后执行如下命令：
```
curl -o $clashdir/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/DNS-Bypass/geo-mode/redir-host-mode/user.yaml && $clashdir/start.sh restart
```
## 3. 重启 Clash 服务
连接 SSH 后，执行如下命令：
```
$clashdir/start.sh restart
```
# 二、 诀窍
## 1. [白名单模式](https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Rule-Templates/geo-mode/template_whitelist.yaml)
若 ShellClash 规则选择的是白名单模式，需要将走直连的所有域名都设置为走国内 DNS 解析，比如我的白名单模式如下：
```
rules:
  - GEOSITE,category-ads-all,⛔️ 广告域名
  - GEOSITE,private,🏠 私有网络
  - GEOSITE,speedtest,📈 网络测速
  - GEOSITE,microsoft@cn,Ⓜ️ Microsoft 中国
  - GEOSITE,apple-cn,🍎 Apple 中国
  - GEOSITE,google-cn,🗽 Google 中国
  - GEOSITE,tld-cn,🇨🇳 国内顶级域名
  - GEOSITE,category-games@cn,🎮 国区游戏
  - GEOSITE,geolocation-!cn,🪜 国外域名
  - GEOSITE,cn,🇨🇳 国内域名
  - GEOIP,telegram,✈️ Telegram IP 地址,no-resolve
  - GEOIP,private,🏠 私有网络,no-resolve
  - GEOIP,cn,🀄 国内 IP 地址
  - MATCH,🐟 漏网之鱼
```
那么需要增加 user.yaml 中的内容：
- 注：兼容 geosite 整合编写功能需要将 Clash.Meta 内核升级到 v1.14.3+版本

```
nameserver:
  - tls://dns.google
  - https://dns.cloudflare.com/dns-query
  - https://doh.opendns.com/dns-query

nameserver-policy:
  'geosite:speedtest,microsoft@cn,apple-cn,google-cn,tld-cn,category-games@cn': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
  'geosite:cn,private': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
```
## 2. [黑名单模式](https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Rule-Templates/geo-mode/template_blacklist.yaml)
若 ShellClash 规则选择的是黑名单模式，需要将走代理的所有域名都设置为走国外 DNS 解析，比如我的黑名单模式如下：
```
rules:
  - GEOSITE,category-ads-all,⛔️ 广告域名
  - GEOSITE,speedtest,📈 网络测速
  - GEOSITE,gfw,🧱 GFWList 域名
  - GEOIP,telegram,✈️ Telegram IP 地址,no-resolve
  - MATCH,🐟 漏网之鱼
```
那么需要增加 user.yaml 中的内容：
- 注：兼容 geosite 整合编写功能需要将 Clash.Meta 内核升级到 v1.14.3+版本

```
nameserver:
  - https://doh.pub/dns-query
  - https://dns.alidns.com/dns-query

nameserver-policy:
  'geosite:gfw': [tls://dns.google, https://dns.cloudflare.com/dns-query, https://doh.opendns.com/dns-query]
```
