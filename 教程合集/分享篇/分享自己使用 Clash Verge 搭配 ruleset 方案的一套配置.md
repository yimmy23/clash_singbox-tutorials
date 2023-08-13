# 分享自己使用 [Clash Verge](https://github.com/zzzgydi/clash-verge) 搭配 ruleset 方案的一套配置
# 声明
1. 此方案采用 `RULE-SET` 规则，属高度定制，仅供参考
2. 规则参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
3. 请根据自身情况进行修改，**适合自己的方案才是最好的方案**，如无特殊需求，可以照搬
4. 此方案适用于 Clash Verge（以 Windows 端为例）
5. 此方案已摒弃 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)，但拦截广告效果依然强劲
# 一、 生成配置文件 .yaml 文件直链
具体方法此处不再赘述，请看《[生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20ruleset%20%E6%96%B9%E6%A1%88.md)》，贴一下我使用的配置：
```
proxy-providers:
  🛫 我的机场:
    type: http
    # 修改为你的 Clash 订阅链接
    url: 'https://example.com/xxx/xxx&flag=clash'
    path: ./proxies/airport.yaml
    interval: 43200
    filter: "香港|台湾|IPV6|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

proxy-groups:
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 6️⃣ IPv6 节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 6️⃣ IPv6 节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 🐟 漏网之鱼, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: ⚡ 直连域名, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🪜 代理域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: 🎮 国区游戏, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: Ⓜ️ Microsoft 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🗽 Google 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🍎 Apple 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🏠 私有网络, type: select, proxies: [🎯 全球直连]}

  - {name: ⛔️ 广告域名, type: select, proxies: [🛑 全球拦截]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  - {name: 🛑 全球拦截, type: select, proxies: [REJECT]}

  # 采用节点负载均衡策略，优点是更稳定，速度可能有提升；推荐在节点复用比较多的情况下使用
  - {name: 🇭🇰 香港节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "台湾"}

  - {name: 6️⃣ IPv6 节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "IPV6"}

  - {name: 🇯🇵 日本节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "美国"}

rule-providers:
  reject:
    type: http
    behavior: domain
    url: 'https://cdn.jsdelivr.net/gh/privacy-protection-tools/anti-AD@master/anti-ad-clash.yaml'
    path: ./ruleset/reject.yaml
    interval: 86400

  lan:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Lan/Lan_No_Resolve.yaml'
    path: ./ruleset/lan.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml'
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
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml'
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
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Proxy/Proxy_Classical.yaml'
    path: ./ruleset/proxy.yaml
    interval: 86400

  direct:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/ChinaMax/ChinaMax_Classical.yaml'
    path: ./ruleset/direct.yaml
    interval: 86400

rules:
  - RULE-SET,reject,⛔️ 广告域名
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
# 二、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)
我用的 Alpha 版，目前一直很稳定  
以管理员身份运行 CMD，执行如下命令：：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %PROGRAMFILES%\Clash Verge\clash-meta.exe -L https://ghproxy.com/https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@release/clash.meta-windows-amd64.exe
```
# 三、 设置部分
1. 设置可参考《[Clash Verge 配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE.md)》，此处只列举配置的不同之处
2. 进入 Clash Verge->配置，点击“新建”（若已有该文件，则忽略此步），类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”
3. 进入文件夹 %USERPROFILE%\\.config\clash-verge\profiles，找到与第 2 步新建的 Merge 文件相对应的 .yaml 文件，复制其文件名并替换下面命令中的{文件名}
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %USERPROFILE%\.config\clash-verge\profiles\{Merge 文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/fake-ip-user.yaml
```
小窍门：
- 1. 可以配置一键更新 Clash.Meta 内核和自定义配置文件的脚本  
- 2. 编辑本文文档，粘贴如下内容：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %PROGRAMFILES%\Clash Verge\clash-meta.exe -L https://ghproxy.com/https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@release/clash.meta-windows-amd64.exe
curl -o %USERPROFILE%\.config\clash-verge\profiles\{Merge 文件名}.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/fake-ip-user.yaml
pause
```
- 3. 另存为 .bat 文件，右击并选择“以管理员身份运行”即可
# 四、 在线 Dashboard 面板
推荐使用在线 Dashboard 面板 [Yacd-meta](https://github.com/MetaCubeX/Yacd-meta)，访问地址：https://yacd.metacubex.one
1. 需要设置该网站“允许不安全内容”，以 Chrome 浏览器为例，进入设置->隐私和安全->网站设置->更多内容设置->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://yacd.metacubex.one`  
<img src="https://user-images.githubusercontent.com/45238096/235448980-52331db5-6b9f-4b0c-a876-1509d34db51a.png" width="60%"/>  

2. 首次进入 https://yacd.metacubex.one 需要添加“API Base URL”，输入 `http://192.168.31.1:9090` 并点击“Add”，最后点击下方新增的 http://192.168.31.1:9090 即可访问 Dashboard 面板  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/086aa876-109a-4a6f-9e9b-22269a869b4f" width="60%"/>
