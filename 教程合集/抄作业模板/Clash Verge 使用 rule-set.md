# 前言：
1. 本模板可以满足 90% 以上的科学上网需求，可以直接套用
2. 本模板适用于 [Clash Verge](https://github.com/zzzgydi/clash-verge)（Windows 端）使用 rule-set 的模式
3. 本模板仅适配 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核
4. 本模板的配置文件请通过 Clash Verge-配置-导入的方式导入（可参考《[生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20ruleset%20%E6%96%B9%E6%A1%88.md)》生成 .yaml 文件直链）
5. 请根据自身选择的 DNS 模式导入相应的 user.yaml 文件
---

# 一、 导入 Clash.Meta 内核
## 1. Release 版  
编辑文本文档，粘贴如下内容：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %PROGRAMFILES%\Clash Verge\clash-meta.exe -L https://ghproxy.com/https://github.com/DustinWin/clash-tools/raw/main/Clash.Meta-release/clash.meta-windows-amd64.exe
```
另存为 .bat 文件，右击并选择以管理员身份运行
## 2. Alpha 版  
编辑文本文档，粘贴如下内容：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %PROGRAMFILES%\Clash Verge\clash-meta.exe -L https://ghproxy.com/https://github.com/DustinWin/clash-tools/releases/download/latest/clash.meta-windows-amd64.exe
```
另存为 .bat 文件，右击并选择以管理员身份运行
# 二、 导入 user.yaml 文件
1. 进入配置，点击“新建”，类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”
2. 进入文件夹 *%USERPROFILE%.config\clash-verge\profiles*，可以看到这里新增了一个.yaml 文件，复制其文件名并替换下面命令中的{文件名}；将下面命令中的{DNS 模式}替换为正在使用的 DNS 模式（fake-ip 或 redir-host）  
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %USERPROFILE%\.config\clash-verge\profiles\{Merge 文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/{DNS 模式}-user.yaml
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

mode: rule
ipv6: true
log-level: silent
allow-lan: true
mixed-port: 19925
unified-delay: false
tcp-concurrent: true
external-controller: 0.0.0.0:9090

find-process-mode: strict
global-client-fingerprint: chrome

tun:
  enable: true
  stack: system
  dns-hijack:
    - 'any:53'
  auto-route: true
  auto-detect-interface: true

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

  - {name: 📥 下载软件, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🏠 私有网络, type: select, proxies: [🎯 全球直连]}

  - {name: ⛔️ 广告域名, type: select, proxies: [🛑 全球拦截]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  - {name: 🛑 全球拦截, type: select, proxies: [REJECT]}

  # 精准筛选出香港节点，支持正则表达式
  - {name: 🇭🇰 香港节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "美国"}

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
    url: 'https://rules.kr328.app/google@cn.yaml'
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
