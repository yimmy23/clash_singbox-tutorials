# 生成带有自定义规则和代理组的配置文件 yaml 直链 geo 方案
- 注：此方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） [路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
# 前言：
1. 本教程可以生成扩展名为 .yaml 文件的直链，可以**一键导入使用了 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核的 Clash 客户端**  
如：[ShellClash](https://github.com/juewuy/ShellClash)、[OpenClash](https://github.com/vernesong/OpenClash) 和 [Clash Verge](https://github.com/zzzgydi/clash-verge) 等
2. 生成的订阅链接地址不会改变，支持更新订阅，**支持国内访问，支持同步机场节点**
3. 生成的订阅链接**自带规则集**，规则集来源 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat)
4. 请先**确定自己机场的订阅链接是否支持 [Clash](https://github.com/Dreamacro/clash/wiki)**，若不支持，可前往 [ACL4SSR 在线订阅转换](https://acl4ssr-sub.github.io)进行生成，参数全部默认即可，生成后的订阅链接需要在末尾添加`&flag=clash`，然后添加到 .yaml 文件中
---
# 一、 注册 [Gist](https://gist.github.com)
进入 https://gist.github.com 网站并注册
# 二、 准备编辑 .yaml 文件
## 1. 打开编辑页面
登录并打开 Gist 可以直接编辑文件，或者鼠标点击页面右上角头像左边的“+”图标新建文件
## 2. 输入描述和完整文件名
“Gist description...”输入描述，随意填写；“Filename including extension...”输入完整文件名**包括扩展名**，如 clashlink.yaml  
<img src="https://user-images.githubusercontent.com/45238096/219593234-64833fcd-5200-4bea-849f-a1865d341fd2.png" width="60%"/>  

# 三、 编辑 .yaml 文件
## 1. 选择规则集模式
① 白名单模式（没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）
```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛫 我的机场 1:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: 'https://example.com/xxx/xxx&flag=clash'
    path: ./proxies/airport1.yaml
    interval: 43200
    # 初步筛选需要的节点，可有效减轻路由器压力，支持正则表达式，不筛选可删除此配置项
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      # 未选择到当前策略组时，不会进行测试，有多个 proxy-providers 时可使用
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

proxy-groups:
  # 手动选择国家或地区节点；根据 proxy-groups 中（下方）国家或地区的节点名称对 proxies 值进行增删改，须一一对应
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  # Speedtest 测速网站：选择“全球直连”为测试本地网络速度（运营商网络速度），可选择其它节点用于测试机场节点速度
  - {name: 📈 网络测速, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

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

  # -----------------国家或地区节点----------------------

  # 自动选择节点，即按照 url 测试结果使用延迟最低的节点；测试后容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试；筛选出“香港”节点，支持正则表达式
  - {name: 🇭🇰 香港节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "美国"}

rules:
  # 自定义规则优先放前面
  - GEOSITE,category-ads-all,⛔️ 广告域名
  # 若使用 Clash Verge，为过滤 P2P 流量（BT 下载），可添加一条 DST-PORT 规则
  - DST-PORT,6881-6889,🎯 全球直连
  - GEOSITE,private,🏠 私有网络
  - GEOSITE,speedtest,📈 网络测速
  - GEOSITE,microsoft@cn,Ⓜ️ Microsoft 中国
  - GEOSITE,apple-cn,🍎 Apple 中国
  - GEOSITE,google-cn,🗽 Google 中国
  - GEOSITE,category-games@cn,🎮 国区游戏
  - GEOSITE,geolocation-!cn,🪜 代理域名
  - GEOSITE,cn,⚡ 直连域名
  - GEOIP,telegram,✈️ Telegram IP
  - GEOIP,private,🏠 私有网络,no-resolve
  - GEOIP,cn,🇨🇳 国内 IP
  - MATCH,🐟 漏网之鱼
```
将模板内容复制到自己 Gist 新建的 .yaml 文件中  
② 黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）
```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛫 我的机场 1:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: 'https://example.com/xxx/xxx=1&flag=clash'
    path: ./proxies/airport1.yaml
    interval: 43200
    # 初步筛选需要的节点，可有效减轻路由器压力，支持正则表达式，不筛选可删除此配置项
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      # 未选择到当前策略组时，不会进行测试，有多个 proxy-providers 时可使用
      lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

  🛫 我的机场 2:
    type: http
    url: 'https://example.com/xxx/xxx=2&flag=clash'
    path: ./proxies/airport2.yaml
    interval: 43200
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

proxy-groups:
  # 手动选择节点；根据 proxy-groups 中（下方）国家或地区的节点名称对 proxies 进行增删改，须一一对应
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  # Speedtest 测速网站：选择“全球直连”为测试本地网络速度（运营商网络速度），可选择其它节点用于测试机场节点速度
  - {name: 📈 网络测速, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - name: 🐟 漏网之鱼, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🧱 GFWList 域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: ✈️ Telegram IP, type: select, proxies: [🚀 节点选择]}

  - {name: ⛔️ 广告域名, type: select, proxies: [🛑 全球拦截]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  - {name: 🛑 全球拦截, type: select, proxies: [REJECT]}

  # -----------------国家或地区节点----------------------

  # 自动选择节点，即按照 url 测试结果使用延迟最低的节点；容差大于 100ms 就会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试；筛选出“香港”节点，支持正则表达式
  - {name: 🇭🇰 香港节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "美国"}

rules:
  # 自定义规则优先放前面
  - GEOSITE,category-ads-all,⛔️ 广告域名
  - GEOSITE,speedtest,📈 网络测速
  - GEOSITE,gfw,🧱 GFWList 域名
  - GEOIP,telegram,✈️ Telegram IP
  - MATCH,🐟 漏网之鱼
```
将模板内容复制到自己 Gist 新建的 .yaml 文件中
## 2. 修改模板
① 首先确定自己机场中有哪些国家或地区的节点，对模板文件中“**proxy-groups**”和“proxy-groups”中“🚀 节点选择”下的“**proxies**”里面的国家或地区进行增删改
- 注：两者中的国家或地区必须一一对应，新增就全部新增，删除就全部删除，修改就全部修改（重要）

② 将“proxy-providers”中的“url”链接改成自己机场的订阅链接（必须支持 Clash，详见《前言：4》）  
③ “proxy-groups”中的“filter”支持[正则表达式](https://tool.oschina.net/regex)，可以精确地筛选出指定的国家或地区节点  
例如：我想筛选出“香港 IPLC”节点，“filter”可以这样写：`filter: "香港.*IPLC|IPLC.*香港"`  
**小窍门：**
- 使用 [ChatGPT](https://chat.openai.com) 查询符合自己要求的正则表达式
- 使用 [New Bing](https://www.bing.com/new) 查询符合自己要求的正则表达式

④ 在“proxy-groups”中“🚀 节点选择”下的“proxies”里，可以将最稳定的节点放在最前面，这样重启路由器后可以自动选择最稳定的节点  
⑤ 在“proxy-groups”中的国家或地区节点里，“type”为“url-test”就是自动选择延迟最低的节点，将“url-test”改成“select”就是手动选择节点  
举个例子：我的机场有 2 个节点，分别是香港节点和日本节点，我想让[哔哩哔哩](https://www.bilibili.com)（B 站）自动选择延迟最低的香港节点，[AcFun](https://www.acfun.cn)（A 站）手动选择日本节点，这个需求怎么写？  
我们可以进入 [v2fly/domain-list-community/data](https://github.com/v2fly/domain-list-community/tree/master/data) 后按 Ctrl+F 组合键搜索“bilibili”和“acfun”，显然可以**精确搜索到结果**，那么就可以这样编写：
- 注：以下只是节选，请酌情套用

```
proxy-groups:
  # 默认选择香港节点
  - {name: 🎥 哔哩哔哩, type: select, proxies: [🇭🇰 香港节点, 🎯 全球直连]}

  # 默认选择日本节点
  - {name: 📽️ AcFun, type: select, proxies: [🇯🇵 日本节点, 🎯 全球直连]}

  # 自动选择延迟最低的香港节点；容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试
  - {name: 🇭🇰 香港节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "香港"}

  # 手动选择日本节点
  - {name: 🇯🇵 日本节点, type: select, use: [🛫 我的机场], filter: "日本"}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

rules:
  # 自定义规则优先放前面
  - GEOSITE,bilibili,🎥 哔哩哔哩
  - GEOSITE,acfun,📽️ AcFun
```
# 四、 生成 .yaml 文件链接
编辑完成后，点击右下角的“Create secret gist”按钮，然后点击右上角的“Raw”按钮  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/ed151cc8-1bae-4b9f-a598-2a2687e8acc5" width="60%"/>  

取出地址栏中的网址，删除后面的一串随机码，**完成后该 .yaml 文件直链才是最终生成的订阅链接**，该订阅链接地址不会改变，在不更改文件名的情况下即使编辑该 .yaml 文件并提交了 n 次也不会改变  
举例，这是原地址：  
`https://gist.githubusercontent.com/DustinWin/f5995e5002fb729380c02dbc38669149/raw/064134b248dddb3a77d85acf45d084751c57ed11/clashlink.yaml`  
删除后面的一串随机码（当前编辑该文件生成的随机码“064134b248dddb3a77d85acf45d084751c57ed11”）  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/12b17493-119d-40b7-8bf6-4d05ceaf7a8c" width="60%"/>  

删除后变成：  
`https://gist.githubusercontent.com/DustinWin/f5995e5002fb729380c02dbc38669149/raw/clashlink.yaml`
# 五、 导入订阅链接
## 1. 在 ShellClash 中导入订阅链接
进入 ShellClash 配置脚本->6->2，粘贴最终生成的订阅链接即可，具体设置请参考《[ShellClash 配置 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE%20geo%20%E6%96%B9%E6%A1%88.md)》
## 2. 在 Clash Verge（Windows 端）中导入订阅链接
进入 Clash Verge->配置->配置文件链接，粘贴最终生成的订阅链接，直接“导入”即可，具体设置请参考《[Clash Verge 配置 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE%20geo%20%E6%96%B9%E6%A1%88.md)》
# 六、 私人定制
到了这里，相信你对里面的机制已经有了一定的认识，那么我们可以对自己的需求进行定制了  
最常见的有：我购买的机场支持[奈飞](https://www.netflix.com)和[亚马逊](https://www.primevideo.com)，但仅新加坡这一个节点支持亚马逊，日本和韩国节点支持奈飞，这个规则怎么写？
1. 进入 [v2fly/domain-list-community/data](https://github.com/v2fly/domain-list-community/tree/master/data) 后按 Ctrl+F 组合键分别搜索“netflix”和“primevideo”
2. 进入 [Loyalsoldier/geoip/text](https://github.com/Loyalsoldier/geoip/tree/release/text) 后按 Ctrl+F 组合键分别搜索“netflix”和“primevideo”
3. 在 v2fly/domain-list-community/data 中可以**精确搜索**到“netflix”和“primevideo”，但在 Loyalsoldier/geoip/text 中只能**精确搜索**到“netflix”，那么我们可以这样编写：
- 注：以下只是节选，请酌情套用

```
proxy-groups:
  # 打开奈飞后手动选择日本或韩国节点
  - {name: 🎥 奈飞节点, type: select, use: [🛫 我的机场], filter: "日本|韩国"}

  # 打开亚马逊后自动选择延迟最低的新加坡节点；容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试
  - {name: 🎞️ 亚马逊节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场], filter: "新加坡"}

rules:
  # 自定义规则优先放前面
  - GEOSITE,netflix,🎥 奈飞节点
  - GEOIP,netflix,🎥 奈飞节点
  - GEOSITE,primevideo,🎞️ 亚马逊节点
```
