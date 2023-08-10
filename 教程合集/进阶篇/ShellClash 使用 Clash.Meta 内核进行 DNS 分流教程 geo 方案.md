# ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程 geo 方案
注：
- 1. 此方案采用 GEOSITE 和 GEOIP 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb）[路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
- 2. 搭配 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 时不要使用该方法 
- 3. DNS 分流简单来说就是**指定国内域名走阿里或腾讯 DNS**，主要是这个配置：
```
nameserver-policy:
  'geosite:cn': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
```
- 4. 所有步骤完成后，请连接 SSH 执行命令 `$clashdir/start.sh restart` 后生效
---
# 一、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)和路由规则文件
可参考《[ShellClash 配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE.md)》里的步骤《一、二》进行操作
# 二、 ShellClash 设置
1. 进入 ShellClash->7 clash 进阶设置->6 配置内置 DNS 服务，将“当前基础 DNS”和“FallbackDNS”都设置为“null”
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/5fb2e16b-5b1d-4393-af3a-fc694b52f4c2" width="60%"/>  

2. 其它设置可参考《[ShellClash 配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE.md)》
# 三、 导入 user.yaml 文件
## 1. DNS 模式为 fake-ip 
该模式不需要使用 DNS 分流，仅配置 `default-nameserver` 和 `nameserver` 这两个参数即可  
此处分享一配置，连接 SSH 后执行如下命令：
```
curl -o $clashdir/yamls/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tutorials@main/dns-bypass/fake-ip-user.yaml && $clashdir/start.sh restart
```
## 2. DNS 模式为 redir-host
该模式需要使用 DNS 分流  
① 白名单模式（没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）  
连接 SSH 后执行命令 `vi $clashdir/yamls/user.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
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
    'geosite:speedtest,microsoft@cn,apple-cn,google-cn,category-games@cn': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
    'geosite:cn,private': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车  
② 黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）  
连接 SSH 后执行命令 `vi $clashdir/yamls/user.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
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
    'geosite:gfw': [tls://dns.google, https://dns.cloudflare.com/dns-query, https://doh.opendns.com/dns-query]
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
