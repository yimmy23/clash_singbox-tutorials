# 生成带有自定义出站和规则的 sing-box 配置文件直链-geodata 方案
- 注：此方案适用于 [sing-box](https://github.com/SagerNet/sing-box)，采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） [路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
# 前言：
1. 本教程可以生成扩展名为 .json 文件的直链，可以**一键导入使用了 sing-box 内核的客户端**
如：[ShellCrash](https://github.com/juewuy/ShellCrash) 和 [sing-box for Android and Apple platforms](https://github.com/SagerNet/sing-box/releases) 等，详见[支持 sing-box 的工具](https://sing-box.sagernet.org/zh/clients)
2. 生成的订阅链接地址不会改变，支持更新订阅，**支持国内访问，支持同步机场节点**
3. 生成的订阅链接**自带规则集**，规则集来源 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat)
4. 本教程必须使用支持 `outbound_providers` 代理集合（即 [Clash](https://github.com/Dreamacro/clash) 订阅链接）的 [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)，请先**确定自己机场的订阅链接是否为 Clash 订阅链接**，若不是，需前往[肥羊在线订阅转换工具](https://suburl.v1.mk)进行转换，“生成类型”选择“Clash”，其它参数保持默认即可，转换后的订阅链接需要在末尾添加 `&flag=clash`，然后添加到 .json 文件 `outbound_providers` 代理集合的 `download_url` 中
5. `outbound_providers` 代理集合配置 `path` 路径后，若路径中含有文件夹如：*./providers/airport.yaml*，需要手动新建此文件夹 *providers* 才能使 .yaml 文件保存到本地，否则将保存到内存中（ 每次启动服务都要重新下载），而在 ShellCrash 中可以连接 SSH 后执行命令 `mkdir -p $CRASHDIR/providers/` 来新建文件夹 *providers*
6. 推荐使用 [Visual Studio Code](https://code.visualstudio.com/Download) 等专业编辑器来修改配置文件
7. ShellCrash 支持本地导入配置文件，可以直接将下方的 .json 直链文件内容复制到 *$CRASHDIR/jsons/config.json* 文件中，可代替通过 ShellCrash 配置脚本->6->2 导入配置文件的方式
---
# 一、 准备编辑 .json 直链文件
## 1. 注册 [Gist](https://gist.github.com)
## 2. 打开编辑页面
登录并打开 Gist 可以直接编辑文件，或者鼠标点击页面右上角头像左边的“+”图标新建文件
## 3. 输入描述和完整文件名
“Gist description...”输入描述，随意填写；“Filename including extension...”输入完整文件名**包括扩展名**，如 singboxlink.json
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/13346166-85cf-474c-9da7-55182e095758" width="60%"/>

# 二、 添加模板和配置文件
## 1. 白名单模式（没有命中规则的网络流量统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）
### ① 添加 .json 模板
将模板内容复制到自己 Gist 新建的 .json 文件中：
```
{
  // 出站
  "outbounds": [
    // 手动选择国家或地区节点；根据“国家或地区出站”的名称对 `outbounds` 值进行增删改，须一一对应
    { "tag": "🚀 节点选择", "type": "selector", "outbounds": [ "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点" ] },
    // Speedtest 测速网站：选择`🎯 全球直连` 为测试本地网络速度（运营商网络速度），可选择其它节点用于测试机场节点速度
    { "tag": "📈 网络测速", "type": "selector", "outbounds": [ "🎯 全球直连", "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点" ] },
    { "tag": "🐟 漏网之鱼", "type": "selector", "outbounds": [ "🚀 节点选择", "🎯 全球直连" ] },
    { "tag": "🔗 直连域名", "type": "selector", "outbounds": [ "🎯 全球直连", "🚀 节点选择" ] },
    { "tag": "🪜 代理域名", "type": "selector", "outbounds": [ "🚀 节点选择", "🎯 全球直连" ] },
    { "tag": "🎮 游戏平台", "type": "selector", "outbounds": [ "🎯 全球直连", "🚀 节点选择" ] },
    { "tag": "Ⓜ️ 微软服务", "type": "selector", "outbounds": [ "🎯 全球直连", "🚀 节点选择" ] },
    { "tag": "📢 谷歌服务", "type": "selector", "outbounds": [ "🎯 全球直连", "🚀 节点选择" ] },
    { "tag": "🍎 苹果服务", "type": "selector", "outbounds": [ "🎯 全球直连", "🚀 节点选择" ] },
    { "tag": "🇨🇳 国内 IP", "type": "selector", "outbounds": [ "🎯 全球直连", "🚀 节点选择" ] },
    { "tag": "📲 电报消息", "type": "selector", "outbounds": [ "🚀 节点选择" ] },
    { "tag": "🔒 私有网络", "type": "selector", "outbounds": [ "🎯 全球直连" ] },
    { "tag": "🛑 广告拦截", "type": "selector", "outbounds": [ "REJECT" ] },
    { "tag": "🎯 全球直连", "type": "selector", "outbounds": [ "DIRECT" ] },
    { "tag": "GLOBAL", "type": "selector", "outbounds": [ "🎯 全球直连", "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点" ] },
    { "tag": "REJECT", "type": "block" },
    { "tag": "DIRECT", "type": "direct" },
    { "tag": "dns-out", "type": "dns" },

    // 单个出站节点（以 vless 为例）
    {
      "tag": "🆓 免费节点",
      "type": "vless",
      "server": "example.com",
      "server_port": 443,
      "uuid": "{uuid}",
      "network": "tcp",
      "tls": { "enabled": true, "server_name": "example.com", "insecure": false },
      "transport": { "type": "ws", "path": "/?ed=2048", "headers": { "Host": "example.com" } }
    },

    // -------------------- 国家或地区出站 --------------------
    // 自动选择节点，即按照 url 测试结果使用延迟最低的节点；测试后容差大于 100ms 才会切换到延迟低的那个节点；筛选出“香港”节点，支持正则表达式
    { "tag": "🇭🇰 香港节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)港|hk|hongkong|hong kong" ] },
    // 若有多个机场，可以使用 `"use_all_providers": true` 代替 `"providers": [ "🛫 我的机场 1", "🛫 我的机场 2", ... ]`
    { "tag": "🇹🇼 台湾节点", "type": "urltest", "tolerance": 100, "use_all_providers": true, "includes": [ "(?i)台|tw|taiwan" ] },
    { "tag": "🇯🇵 日本节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)日本|jp|japan" ] },
    { "tag": "🇸🇬 新加坡节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)新|sg|singapore" ] },
    { "tag": "🇺🇸 美国节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)美|us|unitedstates|united states" ] }
  ],
  // 代理集合（获取机场订阅链接内的所有节点）
  "outbound_providers": [
    {
      "tag": "🛫 我的机场 1",
      "type": "http",
      "healthcheck_url": "https://www.gstatic.com/generate_204",
      "healthcheck_interval": "10m",
      // 机场订阅链接，使用 Clash 链接
      "download_url": "https://example.com/xxx/xxx&flag=clash",
      "path": "./providers/airport1.yaml",
      "download_ua": "clash.meta",
      "download_interval": "24h",
      "download_detour": "DIRECT"
    },
    {
      "tag": "🛫 我的机场 2",
      "type": "http",
      "healthcheck_url": "https://www.gstatic.com/generate_204",
      "healthcheck_interval": "10m",
      "download_url": "https://example.com/xxx/xxx&flag=clash",
      "path": "./providers/airport2.yaml",
      "download_ua": "clash.meta",
      "download_interval": "24h",
      "download_detour": "DIRECT"
    }
  ],
  // 路由
  "route": {
    // 规则
    "rules": [
      { "protocol": [ "dns" ], "outbound": "dns-out" },
      { "clash_mode": "Direct", "outbound": "DIRECT" },
      { "clash_mode": "Global", "outbound": "GLOBAL" },
      // 自定义规则优先放前面
      { "geosite": [ "category-ads-all" ], "outbound": "🛑 广告拦截" },
      // 为过滤 P2P 流量（BT 下载），可添加一条 `port_range` 规则（ShellCrash 会默认开启“只代理常用端口”，可忽略此项）
      { "port_range": [ "6881:6889" ], "outbound": "🎯 全球直连" },
      { "geosite": [ "private" ], "outbound": "🔒 私有网络" },
      { "geosite": [ "microsoft@cn" ], "outbound": "Ⓜ️ 微软服务" },
      { "geosite": [ "apple-cn" ], "outbound": "🍎 苹果服务" },
      { "geosite": [ "google-cn" ], "outbound": "📢 谷歌服务" },
      { "geosite": [ "category-games@cn" ], "outbound": "🎮 游戏平台" },
      { "geosite": [ "speedtest" ], "outbound": "📈 网络测速" },
      { "geosite": [ "geolocation-!cn" ], "outbound": "🪜 代理域名" },
      { "geosite": [ "cn" ], "outbound": "🔗 直连域名" },
      { "geoip": [ "telegram" ], "outbound": "📲 电报消息" },
      { "geoip": [ "private" ],  "outbound": "🔒 私有网络" },
      { "geoip": [ "cn" ], "outbound": "🇨🇳 国内 IP" }
    ],
    // geosite 配置项
    "geosite": {
      "path": "./geodata/geosite.db",
      "download_url": "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.db",
      "download_detour": "DIRECT"
    },
    // geoip 配置项
    "geoip": {
      "path": "./geodata/geoip.db",
      "download_url": "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.db",
      "download_detour": "DIRECT"
    },
    // 默认出站，即没有命中规则的域名或 IP 走该规则
    "final": "🐟 漏网之鱼"
  }
}
```
**贴一张面板效果图（举个例子：我手动选择 `🇹🇼 台湾节点` 策略组，而该策略组是将机场内所有台湾节点按照 url 测试结果自动选择延迟最低的台湾节点）：**  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/1c1a1866-1fc1-4277-92b7-d138e36a4a4b" width="60%"/>
### ② 添加 dns.json
• 作用是使 DNS 能够精确解析国内外域名，国内域名走 realip，国外域名走 fakeip（属 sing-box 内核的核心功能）  
• 连接 SSH 后执行 `vi $CRASHDIR/jsons/dns.json`，按一下 Ins 键（Insert 键），编辑如下内容并粘贴：
```
{
  // DNS
  "dns": {
    // DNS 服务器
    "servers": [
      // 广告 DNS
      { "tag": "dns_block", "address": "rcode://refused" },
      // 阿里 DNS
      { "tag": "dns_alidns", "address": "h3://223.5.5.5/dns-query", "detour": "DIRECT" },
      // 腾讯 DNS
      { "tag": "dns_dnspod", "address": "https://1.12.12.12/dns-query", "detour": "DIRECT" },
      // FakeIP
      { "tag": "dns_fakeip", "address": "fakeip" }
    ],
    // DNS 规则
    "rules": [
      { "outbound": "any", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Direct", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Global", "server": "dns_fakeip", "rewrite_ttl": 1 },
      // geosite.db 规则集文件内必须包含 `category-ads-all` 规则
      { "geosite": [ "category-ads-all" ], "server": "dns_block" },
      // geosite.db 规则集文件内必须包含以下规则
      { "geosite": [ "microsoft@cn", "apple-cn", "google-cn", "category-games@cn", "cn", "private" ], "query_type": [ "A", "AAAA" ], "server": [ "dns_alidns", "dns_dnspod" ] },
      // geosite.db 规则集文件内必须包含 `geolocation-!cn` 规则
      { "geosite": [ "geolocation-!cn" ], "query_type": [ "A", "AAAA" ], "server": "dns_fakeip", "rewrite_ttl": 1 }
    ],
    // 默认 DNS 服务器，即上述 DNS 规则外的域名使用该 DNS 解析
    "final": [ "dns_alidns", "dns_dnspod" ],
    "strategy": "prefer_ipv4",
    "independent_cache": true,
    "reverse_mapping": true,
    "fakeip": { "enabled": true, "inet4_range": "198.18.0.0/15", "inet6_range": "fc00::/18" }
  }
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
## 2. 黑名单模式（只有命中规则的网络流量才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）
### ① 添加 .json 模板
将模板内容复制到自己 Gist 新建的 .json 文件中：
```
{
  // 出站
  "outbounds": [
    // 手动选择国家或地区节点；根据“国家或地区出站”的名称对 `outbounds` 值进行增删改，须一一对应
    { "tag": "🚀 节点选择", "type": "selector", "outbounds": [ "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点" ] },
    // Speedtest 测速网站：选择`🎯 全球直连` 为测试本地网络速度（运营商网络速度），可选择其它节点用于测试机场节点速度
    { "tag": "📈 网络测速", "type": "selector", "outbounds": [ "🎯 全球直连", "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点" ] },
    { "tag": "🐟 漏网之鱼", "type": "selector", "outbounds": [ "🎯 全球直连", "🚀 节点选择" ] },
    { "tag": "🧱 GFWList 域名", "type": "selector", "outbounds": [ "🚀 节点选择", "🎯 全球直连" ] },
    { "tag": "📲 电报消息", "type": "selector", "outbounds": [ "🚀 节点选择" ] },
    { "tag": "🛑 广告拦截", "type": "selector", "outbounds": [ "REJECT" ] },
    { "tag": "🎯 全球直连", "type": "selector", "outbounds": [ "DIRECT" ] },
    { "tag": "GLOBAL", "type": "selector", "outbounds": [ "🎯 全球直连", "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点" ] },
    { "tag": "REJECT", "type": "block" },
    { "tag": "DIRECT", "type": "direct" },
    { "tag": "dns-out", "type": "dns" },

    // 单个出站节点（以 vless 为例）
    {
      "tag": "🆓 免费节点",
      "type": "vless",
      "server": "example.com",
      "server_port": 443,
      "uuid": "{uuid}",
      "network": "tcp",
      "tls": { "enabled": true, "server_name": "example.com", "insecure": false },
      "transport": { "type": "ws", "path": "/?ed=2048", "headers": { "Host": "example.com" } }
    },

    // -------------------- 国家或地区出站 --------------------
    // 自动选择节点，即按照 url 测试结果使用延迟最低的节点；测试后容差大于 100ms 才会切换到延迟低的那个节点；筛选出“香港”节点，支持正则表达式
    { "tag": "🇭🇰 香港节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)港|hk|hongkong|hong kong" ] },
    // 若有多个机场，可以使用 `"use_all_providers": true` 代替 `"providers": [ "🛫 我的机场 1", "🛫 我的机场 2", ... ]`
    { "tag": "🇹🇼 台湾节点", "type": "urltest", "tolerance": 100, "use_all_providers": true, "includes": [ "(?i)台|tw|taiwan" ] },
    { "tag": "🇯🇵 日本节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)日本|jp|japan" ] },
    { "tag": "🇸🇬 新加坡节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)新|sg|singapore" ] },
    { "tag": "🇺🇸 美国节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)美|us|unitedstates|united states" ] }
  ],
  // 代理集合（获取机场订阅链接内的所有节点）
  "outbound_providers": [
    {
      "tag": "🛫 我的机场 1",
      "type": "http",
      "healthcheck_url": "https://www.gstatic.com/generate_204",
      "healthcheck_interval": "10m",
      // 机场订阅链接，使用 Clash 链接
      "download_url": "https://example.com/xxx/xxx&flag=clash",
      "path": "./providers/airport1.yaml",
      "download_ua": "clash.meta",
      "download_interval": "24h",
      "download_detour": "DIRECT"
    },
    {
      "tag": "🛫 我的机场 2",
      "type": "http",
      "healthcheck_url": "https://www.gstatic.com/generate_204",
      "healthcheck_interval": "10m",
      "download_url": "https://example.com/xxx/xxx&flag=clash",
      "path": "./providers/airport2.yaml",
      "download_ua": "clash.meta",
      "download_interval": "24h",
      "download_detour": "DIRECT"
    }
  ],
  // 路由
  "route": {
    // 规则
    "rules": [
      { "protocol": [ "dns" ], "outbound": "dns-out" },
      { "clash_mode": "Direct", "outbound": "DIRECT" },
      { "clash_mode": "Global", "outbound": "🚀 节点选择" },
      // 自定义规则优先放前面
      { "geosite": [ "category-ads-all" ], "outbound": "🛑 广告拦截" },
      { "geosite": [ "speedtest" ], "outbound": "📈 网络测速" },
      { "geosite": [ "gfw" ], "outbound": "🧱 GFWList 域名" },
      { "geoip": [ "telegram" ], "outbound": "📲 电报消息" },
    ],
    // geosite 配置项
    "geosite": {
      "path": "./geodata/geosite.db",
      "download_url": "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.db",
      "download_detour": "DIRECT"
    },
    // geoip 配置项
    "geoip": {
      "path": "./geodata/geoip.db",
      "download_url": "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.db",
      "download_detour": "DIRECT"
    },
    // 默认出站，即没有命中规则的域名或 IP 走该规则
    "final": "🐟 漏网之鱼"
  }
}
```
### ② 添加 dns.json
• 作用是使 DNS 能够精确解析国内外域名，国内域名走 realip，国外域名走 fakeip（属 sing-box 内核的核心功能）  
• 连接 SSH 后执行 `vi $CRASHDIR/jsons/dns.json`，按一下 Ins 键（Insert 键），编辑如下内容并粘贴：
```
{
  // DNS
  "dns": {
    // DNS 服务器
    "servers": [
      // 广告 DNS
      { "tag": "dns_block", "address": "rcode://refused" },
      // 阿里 DNS
      { "tag": "dns_alidns", "address": "h3://223.5.5.5/dns-query", "detour": "DIRECT" },
      // 腾讯 DNS
      { "tag": "dns_dnspod", "address": "https://1.12.12.12/dns-query", "detour": "DIRECT" },
      // FakeIP
      { "tag": "dns_fakeip", "address": "fakeip" }
    ],
    // DNS 规则
    "rules": [
      { "outbound": "any", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Direct", "server": [ "dns_alidns", "dns_dnspod" ] },
      { "clash_mode": "Global", "server": "dns_fakeip", "rewrite_ttl": 1 },
      // geosite.db 规则集文件内必须包含 `category-ads-all` 规则
      { "geosite": [ "category-ads-all" ], "server": "dns_block" },
      // geosite.db 规则集文件内必须包含 `gfw` 规则
      { "geosite": [ "gfw" ], "query_type": [ "A", "AAAA" ], "server": "dns_fakeip", "rewrite_ttl": 1 }
    ],
    // 默认 DNS 服务器，即上述 DNS 规则外的域名使用该 DNS 解析
    "final": [ "dns_alidns", "dns_dnspod" ],
    "strategy": "prefer_ipv4",
    "independent_cache": true,
    "reverse_mapping": true,
    "fakeip": { "enabled": true, "inet4_range": "198.18.0.0/15", "inet6_range": "fc00::/18" }
  }
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
# 三、 修改模板和配置文件
1. 将代理集合 `outbound_providers` 中的 `download_url` 链接改成自己机场的订阅链接（必须为 Clash 订阅链接，详见《前言：4》）
2. 确定自己机场中有哪些国家或地区的节点，然后对模板文件里 `outbounds` 中的“**国家或地区出站**”以及 `🚀 节点选择`、`📈 网络测速` 和 `GLOBAL` 下的 `outbounds` 里面的国家或地区进行增删改
- 注：两者中的国家或地区必须一一对应，新增就全部新增，删除就全部删除，修改就全部修改（重要）

3. 在 `🚀 节点选择` 出站下的 `outbounds` 里，可以将最稳定的节点放在最前面，配置完成后会自动选择最稳定的节点
4. 在“国家或地区出站”里，`type` 为 `urltest` 就是自动选择延迟最低的节点，将 `urltest` 改成 `selector` 就是手动选择节点
举个例子：我的机场有 2 个节点，分别是香港节点和日本节点，我想让[哔哩哔哩](https://www.bilibili.com)（B 站）自动选择延迟最低的香港节点，[AcFun](https://www.acfun.cn)（A 站）可以手动选择日本节点，这个需求怎么写？
我们可以进入 [v2fly/domain-list-community/data](https://github.com/v2fly/domain-list-community/tree/master/data) 后按 Ctrl+F 组合键搜索“bilibili”和“acfun”，显然可以**精确搜索到结果**，那么就可以这样编写：
- 注：以下只是节选，请酌情套用

```
{
  // 出站
  "outbounds": [
    // 默认选择香港节点
    { "tag": "📺 哔哩哔哩", "type": "selector", "outbounds": [ "🇭🇰 香港节点", "🎯 全球直连" ] },
    // 默认选择日本节点
    { "tag": "📽️ AcFun", "type": "selector", "outbounds": [ "🇯🇵 日本节点", "🎯 全球直连" ] },
    // 自动选择延迟最低的香港节点；容差大于 100ms 才会切换到延迟低的那个节点
    { "tag": "🇭🇰 香港节点", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)港|hk|hongkong|hong kong" ] },
    // 手动选择日本节点
    { "tag": "🇯🇵 日本节点", "type": "selector", "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)日本|jp|japan" ] },
    { "tag": "🎯 全球直连", "type": "selector", "outbounds": [ "DIRECT" ] },
    { "tag": "DIRECT", "type": "direct" }
  ],
  // 路由
  "route": {
    // 规则
    "rules": [
      // 自定义规则优先放前面
      { "geosite": [ "bilibili" ], "outbound": "📺 哔哩哔哩" },
      { "geosite": [ "acfun" ], "outbound": "📽️ AcFun" }
    ]
  }
}
```
# 四、 生成 .json 文件链接
编辑完成后，点击右下角的“Create secret gist”按钮，然后点击右上角的“Raw”按钮  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/de55458c-b403-438c-adf9-2503f1aad71f" width="60%"/>

取出地址栏中的网址，删除后面的一串随机码，**完成后该 .json 文件直链才是最终生成的订阅链接**，该订阅链接地址不会改变，在不更改文件名的情况下即使编辑该 .json 文件并提交了 n 次也不会改变
举例，这是原地址：
`https://gist.githubusercontent.com/DustinWin/40c0611fda5d6fcd0795ee5a15de7c73/raw/b3c63051134510ae9825068bbcf5219817761f57/singboxlink.json`
删除后面的一串随机码（当前编辑该文件生成的随机码“b3c63051134510ae9825068bbcf5219817761f57”）  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/a409ace5-8f03-4776-b9e3-dce3ba804844" width="60%"/>

删除后变成：
`https://gist.githubusercontent.com/DustinWin/40c0611fda5d6fcd0795ee5a15de7c73/raw/singboxlink.json`
# 五、 导入订阅链接（以 ShellCrash 导入订阅链接为例）
进入 ShellCrash 配置脚本->6->2，粘贴最终生成的订阅链接即可，具体设置请参考《[ShellCrash 配置-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md)》
# 六、 私人定制
到了这里，相信你对里面的机制已经有了一定的认识，那么我们可以对自己的需求进行定制了
最常见的有：我购买的机场支持[奈飞](https://www.netflix.com)和[亚马逊](https://www.primevideo.com)，但仅新加坡这一个节点支持亚马逊，日本和韩国节点支持奈飞，这个规则怎么写？
1. 进入 [v2fly/domain-list-community/data](https://github.com/v2fly/domain-list-community/tree/master/data) 后按 Ctrl+F 组合键分别搜索“netflix”和“primevideo”
2. 进入 [Loyalsoldier/geoip/text](https://github.com/Loyalsoldier/geoip/tree/release/text) 后按 Ctrl+F 组合键分别搜索“netflix”和“primevideo”
3. 在 v2fly/domain-list-community/data 中可以**精确搜索**到“netflix”和“primevideo”，但在 Loyalsoldier/geoip/text 中只能**精确搜索**到“netflix”，那么我们可以这样编写：
- 注：以下只是节选，请酌情套用

```
{
  // 出站
  "outbounds": [
    // 打开奈飞后手动选择日本或韩国节点
    { "tag": "🎥 奈飞视频", "type": "selector", "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)日本|jp|japan|韩|kr|korea" ] },
    // 打开亚马逊后自动选择延迟最低的新加坡节点；容差大于 100ms 才会切换到延迟低的那个节点
    { "tag": "🎬 Prime Video", "type": "urltest", "tolerance": 100, "providers": [ "🛫 我的机场 1", "🛫 我的机场 2" ], "includes": [ "(?i)新|sg|singapore" ] }
  ],
  // 路由
  "route": {
    // 规则
    "rules": [
      // 自定义规则优先放前面
      { "geosite": [ "netflix" ], "outbound": "🎥 奈飞视频" },
      { "geoip": [ "netflix" ], "outbound": "🎥 奈飞视频" }
      { "geosite": [ "primevideo" ], "outbound": "🎬 Prime Video" }
    ]
  }
}
```
