# Clash.Meta for Android 推荐设置
注：
- 1. 考虑到 Clash.Meta for Android 的特殊性，强烈建议使用 ruleset 方案并使用 redir-host 模式（由 fake-ip 模式模拟）
- 2. 此方案采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
  3. `RULE-SET` 规则参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
# 一、 生成带有自定义规则和代理组的配置文件 yaml 直链
具体方法请参考《[生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案](https://www.baidu.com)》  
配置文件内容如下：
## 1. 白名单模式（没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）
```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛫 我的机场 1:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: 'https://example.com/xxx/xxx&flag=clash'
    path: ./proxies/airport1.yaml
    interval: 43200
    # 初步筛选需要的节点，支持正则表达式，不筛选可删除此配置项
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      # 未选择到当前策略组时，不会进行测试
      lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

  🛫 我的机场 2:
    type: http
    url: 'https://example.com/xxx/xxx&flag=clash'
    path: ./proxies/airport2.yaml
    interval: 43200
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

mode: rule
ipv6: true
log-level: silent
allow-lan: true
mixed-port: 7890
unified-delay: false
tcp-concurrent: true
external-controller: 127.0.0.1:9090

find-process-mode: strict
global-client-fingerprint: chrome

profile:
  store-selected: true
  store-fake-ip: true

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  use-hosts: true
  fake-ip-range: 198.18.0.1/16
  # 使用 fake-ip 模式模拟 redir-host 模式
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  default-nameserver: [https://1.12.12.12/dns-query, https://223.5.5.5/dns-query]
  nameserver: [tls://dns.google, https://dns.cloudflare.com/dns-query, https://doh.opendns.com/dns-query]
  proxy-server-nameserver: [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
  nameserver-policy:
    'rule-set:networktest,microsoft-cn,apple-cn,google-cn,games-cn': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
    'rule-set:direct,lan': [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]

proxy-groups:
  # 手动选择国家或地区节点；根据 proxy-groups 中（下方）国家或地区的节点名称对 proxies 值进行增删改，须一一对应
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  # Speedtest 测速网站：选择“全球直连”为测试本地网络速度（运营商网络速度），可选择其它节点用于测试机场节点速度
  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 🐟 漏网之鱼, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: ⚡ 直连域名, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🪜 代理域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: 🎮 国区游戏, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: Ⓜ️ Microsoft 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🗽 Google 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🍎 Apple 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 📥 下载软件, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🏠 私有网络, type: select, proxies: [🎯 全球直连]}

  - {name: ⛔️ 广告域名, type: select, proxies: [🛑 全球拦截]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  - {name: 🛑 全球拦截, type: select, proxies: [REJECT]}

  # -----------------国家或地区节点----------------------

  # 自动选择节点，即按照 url 测试结果使用延迟最低的节点；测试后容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试；筛选出“香港”节点，支持正则表达式
  - {name: 🇭🇰 香港节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "美国"}

# 规则集 .yaml 文件；每天自动更新
rule-providers:
  reject:
    type: http
    behavior: domain
    url: 'https://fastly.jsdelivr.net/gh/privacy-protection-tools/anti-AD@master/anti-ad-clash.yaml'
    path: ./ruleset/reject.yaml
    interval: 86400

  applications:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt'
    path: ./ruleset/applications.yaml
    interval: 86400

  lan:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Lan/Lan_No_Resolve.yaml'
    path: ./ruleset/lan.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml'
    path: ./ruleset/networktest.yaml
    interval: 86400

  microsoft-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/microsoft@cn.yaml'
    path: ./ruleset/microsoft-cn.yaml
    interval: 86400

  apple-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/apple@cn.yaml'
    path: ./ruleset/apple-cn.yaml
    interval: 86400

  google-cn:
    type: http
    behavior: domain
    url: 'https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml'
    path: ./ruleset/google-cn.yaml
    interval: 86400

  games-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/category-games@cn.yaml'
    path: ./ruleset/games-cn.yaml
    interval: 86400

  proxy:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Proxy/Proxy_Classical.yaml'
    path: ./ruleset/proxy.yaml
    interval: 86400

  direct:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/ChinaMax/ChinaMax_Classical.yaml'
    path: ./ruleset/direct.yaml
    interval: 86400

rules:
  # 自定义规则优先放前面
  - RULE-SET,reject,⛔️ 广告域名
  - RULE-SET,applications,📥 下载软件
  - RULE-SET,lan,🏠 私有网络
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,microsoft-cn,Ⓜ️ Microsoft 中国
  - RULE-SET,apple-cn,🍎 Apple 中国
  - RULE-SET,google-cn,🗽 Google 中国
  - RULE-SET,games-cn,🎮 国区游戏
  - RULE-SET,proxy,🪜 代理域名
  - RULE-SET,direct,⚡ 直连域名
  - MATCH,🐟 漏网之鱼
```
## 2. 黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）
```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛫 我的机场 1:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: 'https://example.com/xxx/xxx&flag=clash'
    path: ./proxies/airport1.yaml
    interval: 43200
    # 初步筛选需要的节点，支持正则表达式，不筛选可删除此配置项
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      # 未选择到当前策略组时，不会进行测试
      lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

  🛫 我的机场 2:
    type: http
    url: 'https://example.com/xxx/xxx&flag=clash'
    path: ./proxies/airport2.yaml
    interval: 43200
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

mode: rule
ipv6: true
log-level: silent
allow-lan: true
mixed-port: 7890
unified-delay: false
tcp-concurrent: true
external-controller: 127.0.0.1:9090

find-process-mode: strict
global-client-fingerprint: chrome

profile:
  store-selected: true
  store-fake-ip: true

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  use-hosts: true
  fake-ip-range: 198.18.0.1/16
  # 使用 fake-ip 模式模拟 redir-host 模式
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  default-nameserver: [https://1.12.12.12/dns-query, https://223.5.5.5/dns-query]
  nameserver: [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
  proxy-server-nameserver: [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
  nameserver-policy:
    'rule-set:proxy': [tls://dns.google, https://dns.cloudflare.com/dns-query, https://doh.opendns.com/dns-query]

proxy-groups:
  # 手动选择国家或地区节点；根据 proxy-groups 中（下方）国家或地区的节点名称对 proxies 值进行增删改，须一一对应
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  # Speedtest 测速网站：选择“全球直连”为测试本地网络速度（运营商网络速度），可选择其它节点用于测试机场节点速度
  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 🐟 漏网之鱼, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🪜 代理域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: 📥 下载软件, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🏠 私有网络, type: select, proxies: [🎯 全球直连]}

  - {name: ⛔️ 广告域名, type: select, proxies: [🛑 全球拦截]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  - {name: 🛑 全球拦截, type: select, proxies: [REJECT]}

  # -----------------国家或地区节点----------------------

  # 自动选择节点，即按照 url 测试结果使用延迟最低的节点；测试后容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试；筛选出“香港”节点，支持正则表达式
  - {name: 🇭🇰 香港节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "美国"}

# 规则集 .yaml 文件；每天自动更新
rule-providers:
  reject:
    type: http
    behavior: domain
    url: 'https://fastly.jsdelivr.net/gh/privacy-protection-tools/anti-AD@master/anti-ad-clash.yaml'
    path: ./ruleset/reject.yaml
    interval: 86400

  applications:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt'
    path: ./ruleset/applications.yaml
    interval: 86400

  lan:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Lan/Lan_No_Resolve.yaml'
    path: ./ruleset/lan.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml'
    path: ./ruleset/networktest.yaml
    interval: 86400

  proxy:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Proxy/Proxy_Classical.yaml'
    path: ./ruleset/proxy.yaml
    interval: 86400

rules:
  # 自定义规则优先放前面
  - RULE-SET,reject,⛔️ 广告域名
  - RULE-SET,applications,📥 下载软件
  - RULE-SET,lan,🏠 私有网络
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,proxy,🪜 代理域名
  - MATCH,🐟 漏网之鱼
```
# 二、 导入配置文件并启动 Clash
1. 进入 Clash Meta for Android->配置->创建配置->从 URL 导入，“URL”输入《[第一步](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash.Meta%20for%20Android%20%E6%8E%A8%E8%8D%90%E8%AE%BE%E7%BD%AE.md#%E4%B8%80-%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6-yaml-%E7%9B%B4%E9%93%BE)》中生成的配置文件 yaml 直链，“自动更新”填写“1440”，最后点击右上角的“保存图标”
2. 返回到主界面，点击“点此启用”即可启动 Clash 服务
