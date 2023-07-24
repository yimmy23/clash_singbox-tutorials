# 生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案
此方案采用 `RULE-SET` 规则
# 前言：
1. 本教程可以生成扩展名为.yaml 文件的直链，可以**一键导入使用了 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核的 Clash 客户端**  
如：[ShellClash](https://github.com/juewuy/ShellClash)、[OpenClash](https://github.com/vernesong/OpenClash)、[Clash Verge](https://github.com/zzzgydi/clash-verge) 和 [Clash.Meta for Android](https://github.com/MetaCubeX/ClashMetaForAndroid) 等
2. 生成的订阅链接地址不会改变，支持更新订阅，**支持国内访问，支持同步机场节点**
3. 生成的订阅链接**自带规则集**，规则集来源 [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)
4. 强烈建议生成订阅链接后先导入 [Clash Verge](https://github.com/zzzgydi/clash-verge/releases) 进行测试，**测试通过后再导入 ShellClash 内**
5. 请先**确定自己机场的订阅链接是否支持 [Clash](https://github.com/Dreamacro/clash/wiki)**，若不支持，可前往 [ACL4SSR 在线订阅转换](https://acl4ssr-sub.github.io)进行生成，参数全部默认即可，生成后的订阅链接需要在末尾添加`&flag=clash`，然后添加到.yaml 文件中
---
# 一、 注册 Gist
进入 https://gist.github.com 网站并注册
# 二、 准备编辑.yaml 文件
## 1. 打开编辑页面
登录并打开 [Gist](https://gist.github.com) 可以直接编辑文件，或者鼠标点击页面右上角头像左边的“+”图标新建文件
## 2. 输入描述和完整文件名
“Gist description...”输入描述，随意填写；“Filename including extension...”输入完整文件名**包括扩展名**，如 clashlink.yaml  
![QQ截图20230217162956](https://user-images.githubusercontent.com/45238096/219593234-64833fcd-5200-4bea-849f-a1865d341fd2.png)  

# 三、 编辑.yaml 文件
## 1. 选择规则集模式
① 白名单模式（没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）  
[点击下载白名单模式模板文件](https://cdn.jsdelivr.net/gh/DustinWin/clash-tutorials@main/rule-templates/ruleset-mode/template_whitelist.yaml)，将模板文件中的所有内容复制到自己 Gist 新建的.yaml 文件中  
② 黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）  
[点击下载黑名单模式模板文件](https://cdn.jsdelivr.net/gh/DustinWin/clash-tutorials@main/rule-templates/ruleset-mode/template_blacklist.yaml)，将模板文件中的所有内容复制到自己 Gist 新建的.yaml 文件中
## 2. 修改模板
① 首先确定自己机场中有哪些国家或地区的节点，对模板文件中“**proxy-groups**”和“proxy-groups”中“🚀 节点选择”下的“**proxies**”里面的国家或地区进行增删改
- 注：两者中的国家或地区必须一一对应，新增就全部新增，删除就全部删除，修改就全部修改（重要）

② 将“proxy-providers”中的“url”链接改成自己机场的订阅链接（必须支持 Clash，详见《前言：5》）  
③ “proxy-groups”中的“filter”支持[正则表达式](https://tool.oschina.net/regex)，可以精确地筛选出指定的国家或地区节点  
例如：我想筛选出“香港 IPLC”节点，“filter”可以这样写：
`filter: "香港.*IPLC|IPLC.*香港"`  
**小窍门：**
- 使用 [ChatGPT](https://chat.openai.com) 查询符合自己要求的正则表达式
- 使用 [New Bing](https://www.bing.com/new) 查询符合自己要求的正则表达式

④ 在“proxy-groups”中“🚀 节点选择”下的“proxies”里，可以将最稳定的节点放在最前面，这样重启路由器后可以自动选择最稳定的节点  
⑤ 在“proxy-groups”中的国家或地区节点里，“type”为“url-test”就是自动选择延迟最低的节点，将“url-test”改成“select”就是手动选择节点  
举个例子：我的机场有 2 个节点，分别是香港节点和日本节点，我想让[哔哩哔哩](https://www.bilibili.com)（B 站）自动选择延迟最低的香港节点，[AcFun](https://www.acfun.cn)（A 站）手动选择日本节点，这个需求怎么写？  
我们可以进入 [blackmatrix7/ios_rule_script/rule/Clash](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash) 后按 Ctrl+F 组合键搜索“bilibili”和“acfun”，显然可以**精确搜索到结果**  
进入指定目录并点击 yaml 文件，然后点击“Raw”获取下载地址，并将下载地址[转换为 CDN 链接](https://www.jsdelivr.com/github)，那么就可以这样编写：
- 注：以下只是节选，请酌情套用

```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛫 我的机场:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: 'https://example.com/xxx/clash'
    path: ./proxies/airport.yaml
    interval: 86400
    # 筛选出需要的节点，支持正则表达式，不筛选可删除此配置项
    filter: "香港|日本"
    health-check:
      enable: true
      # 未选择到当前策略组时，不会进行测试
      # lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

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

rule-providers:
  bilibili:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/BiliBili/BiliBili.yaml'
    path: ./ruleset/bilibili.yaml
    interval: 86400

  acfun:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/AcFun/AcFun.yaml'
    path: ./ruleset/acfun.yaml
    interval: 86400

rules:
  # 自定义规则优先放前面
  - RULE-SET,bilibili,🎥 哔哩哔哩
  - RULE-SET,acfun,📽️ AcFun
```
# 四、 生成.yaml 文件链接
## 1. 生成链接
编辑完成后，点击右下角的“Create secret gist”按钮，然后点击右上角的“Raw”按钮  
![微信截图_20230423201617](https://user-images.githubusercontent.com/45238096/233839417-51d1ab65-3e6b-4ae8-bf01-9d342029b423.png)  

## 2. 修改链接
取出地址栏中的网址，删除后面的一串随机码，**完成后该.yaml 文件直链才是最终生成的订阅链接**，该订阅链接地址不会改变，在不更改文件名的情况下即使编辑该.yaml 文件并提交了 n 次也不会改变  
举例，这是原地址：  
`https://gist.githubusercontent.com/DustinWin/a6d67d1c2c5da5ece004efcd791e4bf4/raw/df770aae2001b2eab426a385ea10bbbb35a35c52/template_whitelist.yaml`  
删除后面的一串随机码（当前编辑该文件生成的随机码“df770aae2001b2eab426a385ea10bbbb35a35c52”）  
![QQ截图20230323121300](https://user-images.githubusercontent.com/45238096/227101230-83e6ee64-ebde-4045-acdc-1dc1eb5d9b4d.png)  

删除后变成：  
`https://gist.githubusercontent.com/DustinWin/a6d67d1c2c5da5ece004efcd791e4bf4/raw/template_whitelist.yaml`  
# 五、 导入订阅链接
## 1. 在 ShellClash 中导入订阅链接  
进入 ShellClash 配置脚本，选择 6-2，直接粘贴最终生成的订阅链接即可
## 2. 在各个平台的 Clash 客户端中导入订阅链接  
粘贴最终生成的订阅链接，直接“导入”（Import）即可
# 六、 机场订阅链接改变或者更换了机场
## 1. 修改.yaml 文件
直接编辑该.yaml 文件并**重复《三、 2》中的步骤**
## 2. 更新订阅
① 在 ShellClash 中更新订阅  
进入 ShellClash 配置脚本，选择 6-4 手动更新或 5-5 添加定时更新，也可连接 SSH 后执行如下命令进行更新：
```
$clashdir/start.sh getyaml
```
② 在各个平台的 Clash 客户端中更新订阅  
一般进入“配置”（Profiles），然后点击“更新”（Update）即可
## 3. 更新机场节点
在 ShellClash Dashboard 面板（进入“代理” Proxies）和各个平台的 Clash 客户端中更新 Proxy Provider 即可
## 4. 更新规则
在 ShellClash Dashboard 面板（进入“规则” Rules）和各个平台的 Clash 客户端中更新 Rule Provider 即可
# 七、 私人定制
到了这里，相信你对里面的机制已经有了一定的认识，那么我们可以对自己的需求进行定制了  
最常见的有：我购买的机场支持[奈飞](https://www.netflix.com)和[亚马逊](https://www.primevideo.com)，但仅新加坡这一个节点支持亚马逊，日本和韩国节点支持奈飞，这个规则怎么写？
1. 进入 [blackmatrix7/ios_rule_script/rule/Clash](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash) 后按 Ctrl+F 组合键分别搜索“netflix”和“primevideo”
2. 可以**精确搜索**到“netflix”和“primevideo”
3. 进入指定目录并点击 yaml 文件，然后点击“Raw”获取下载地址，并将下载地址[转换为 CDN 链接](https://www.jsdelivr.com/github)，那么就可以这样编写：
- 注：以下只是节选，请酌情套用

```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛫 我的机场:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: 'https://example.com/xxx/clash'
    path: ./proxies/airport.yaml
    interval: 86400
    # 筛选出需要的节点，支持正则表达式，不筛选可删除此配置项
    filter: "日本|韩国|新加坡"
    health-check:
      enable: true
      # 未选择到当前策略组时，不会进行测试
      # lazy: true
      url: 'https://www.gstatic.com/generate_204'
      interval: 600

proxy-groups:
  # 打开奈飞后手动选择日本或韩国节点
  - {name: 🎥 奈飞节点, type: select, use: [🛫 我的机场], filter: "日本|韩国"}

  # 打开亚马逊后自动选择延迟最低的新加坡节点；容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试
  - {name: 🎞️ 亚马逊节点, type: url-test, tolerance: 100, use: [🛫 我的机场], filter: "新加坡"}

rule-providers:
  netflix:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Netflix/Netflix.yaml'
    path: ./ruleset/netflix.yaml
    interval: 86400

  primevideo:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/PrimeVideo/PrimeVideo.yaml'
    path: ./ruleset/primevideo.yaml
    interval: 86400

rules:
  # 自定义规则优先放前面
  - RULE-SET,netflix,🎥 奈飞节点
  - RULE-SET,primevideo,🎞️ 亚马逊节点
```
