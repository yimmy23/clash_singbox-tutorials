## 前言：
1. 本模板可以满足 90% 以上的科学上网需求，可以直接套用
2. 本模板适用于 ShellClash 使用 geo 规则集文件（geosite.dat、geoip.dat 和 Country.mmdb）的模式
3. 本模板的配置文件请通过 ShellClash-6-2 的方式导入（可参考《[生成带有自定义规则和代理组的配置文件 yaml 直链 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geo%20%E6%96%B9%E6%A1%88.md)》生成 .yaml 文件直链）
4. 请根据自身选择的 DNS 模式导入相应的 user.yaml 文件
---
# 一、 导入 geo 规则集文件
连接 SSH 后执行如下命令：
```
curl -o $clashdir/GeoSite.dat -L https://github.com/DustinWin/clash-geosite/releases/download/latest/geosite.dat
curl -o $clashdir/GeoIP.dat -L https://github.com/DustinWin/clash-geoip/releases/download/latest/geoip.dat
curl -o $clashdir/Country.mmdb -L https://github.com/DustinWin/clash-geoip/releases/download/latest/Country.mmdb
```
# 二、 导入 user.yaml 文件
## 1. DNS 模式为 fake-ip
连接 SSH 后执行如下命令：
```
curl -o $clashdir/yamls/user.yaml -L https://github.com/DustinWin/clash-geosite/releases/download/latest/fake-ip-user.yaml
```
## 2. DNS 模式为 redir-host
连接 SSH 后执行如下命令：
```
curl -o $clashdir/yamls/user.yaml -L https://github.com/DustinWin/clash-geosite/releases/download/latest/redir-host-user.yaml
```
# 三、 导入配置文件
```
proxy-providers:
  🛫 我的机场:
    type: http
    # 此处填写你的机场 Clash 订阅链接
    url: 'https://example.com/xxx/xxx?&flag=clash'
    path: ./proxies/airport.yaml
    interval: 43200
    # 对机场节点进行初步筛选，支持正则表达式
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

proxy-groups:
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🐟 漏网之鱼, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: ⚡ 直连域名, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🪜 代理域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: 🎮 国区游戏, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: Ⓜ️ Microsoft 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🗽 Google 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🍎 Apple 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🇨🇳 国内 IP, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: ✈️ Telegram IP, type: select, proxies: [🚀 节点选择]}

  - {name: 🏠 私有网络, type: select, proxies: [🎯 全球直连]}

  - {name: ⛔️ 广告域名, type: select, proxies: [🛑 全球拦截]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  - {name: 🛑 全球拦截, type: select, proxies: [REJECT]}

  # 精准筛选出香港节点，支持正则表达式
  - {name: 🇭🇰 香港游戏节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "美国"}

rules:
 - GEOSITE,ads,⛔️ 广告域名
 - GEOSITE,lan,🏠 私有网络
 - GEOSITE,networktest,📈 网络测试
 - GEOSITE,microsoft-cn,Ⓜ️ Microsoft 中国
 - GEOSITE,apple-cn,🍎 Apple 中国
 - GEOSITE,google-cn,🗽 Google 中国
 - GEOSITE,games-cn,🎮 国区游戏
 - GEOSITE,proxy,🪜 代理域名
 - GEOSITE,cn,⚡ 直连域名
 - GEOIP,telegram,✈️ Telegram IP
 - GEOIP,lanip,🏠 私有网络,no-resolve
 - GEOIP,cn,🇨🇳 国内 IP
 - MATCH,🐟 漏网之鱼
```
