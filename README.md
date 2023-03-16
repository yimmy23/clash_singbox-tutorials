**[全网最详细的解锁 SSH ShellClash 搭配 AdGuardHome 安装和配置教程](https://github.com/DustinWin/Router-Plugins/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellClash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.md)**  
**[ShellClash 和 AdGuardHome 快速安装教程](https://github.com/DustinWin/Router-Plugins/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellClash%20%E5%92%8C%20AdGuardHome%20%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.md)**  
**[ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程](https://github.com/DustinWin/Router-Plugins/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B.md)**  
**[本地配置自定义规则和代理组](https://github.com/DustinWin/Router-Plugins/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84.md)**
# 生成带有自定义规则和代理组的配置文件.yaml 直链
# 前言：
1. 本教程可以生成扩展名为.yaml 文件的直链，可以**一键导入使用了 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核的 Clash 客户端**  
如：[ShellClash](https://github.com/juewuy/ShellClash)、[OpenClash](https://github.com/vernesong/OpenClash)、[Clash Verge](https://github.com/zzzgydi/clash-verge) 和 [Clash.Meta for Android](https://github.com/MetaCubeX/ClashMetaForAndroid) 等
2. 生成的订阅链接地址不会改变，支持更新订阅，**支持国内访问，支持同步机场节点**
3. 生成的订阅链接**自带规则集**，规则集参考 https://github.com/Loyalsoldier/clash-rules
4. 强烈建议生成订阅链接后先导入 [Clash Verge](https://github.com/zzzgydi/clash-verge/releases) 进行测试，**测试通过后再导入 ShellClash 内**
5. 请先**确定自己机场的订阅链接是否支持 [Clash](https://github.com/Dreamacro/clash/wiki)**，若不支持，可前往 [ACL4SSR 在线订阅转换](https://acl4ssr-sub.github.io)进行生成，参数全部默认即可，再将生成后的订阅链接添加到.yaml 文件中
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
[点击下载白名单模式模板文件](https://cdn.jsdelivr.net/gh/DustinWin/Router-Plugins@main/Rule-Templates/template_whitelist.yaml)，将模板文件中的所有内容复制到自己 Gist 新建的.yaml 文件中  
② 黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）  
[点击下载黑名单模式模板文件](https://cdn.jsdelivr.net/gh/DustinWin/Router-Plugins@main/Rule-Templates/template_blacklist.yaml)，将模板文件中的所有内容复制到自己 Gist 新建的.yaml 文件中
## 2. 修改模板
① 首先确定自己机场中有哪些国家或地区的节点，对模板文件中“**proxy-groups**”和“proxy-groups”中“🚀 节点选择”下的“**proxies**”里面的国家或地区进行增删改
- 注：两者中的国家或地区必须一一对应，新增就全部新增，删除就全部删除，修改就全部修改（重要）

② 将“proxy-providers”中的“url”链接改成自己机场的订阅链接（必须支持 Clash，详见《前言：5》）  
③ “proxy-groups”中的“filter”支持[正则表达式](https://tool.oschina.net/regex)，可以精确地筛选出指定的国家或地区节点  
例如：我想筛选出“香港 IPLC”节点，“filter”可以这样写：
`filter: "香港.*IPLC|IPLC.*香港"`  
**小窍门：**
- 使用 [ChatGPT](https://chat.openai.com/chat) 查询符合自己要求的正则表达式
- 使用 [New Bing](https://www.bing.com/new) 查询符合自己要求的正则表达式

④ 在“proxy-groups”中“🚀 节点选择”下的“proxies”里，可以将最稳定的节点放在最前面，这样重启路由器后可以自动选择最稳定的节点  
⑤ 在“proxy-groups”中的国家或地区节点里，“type”为“url-test”就是自动选择延迟最低的节点，将“url-test”改成“select”就是手动选择节点  
举个例子：我的机场有 2 个节点，分别是香港节点和日本节点，我想自动选择延迟最低的香港节点，手动选择日本节点，这个需求怎么写？ 
- 注：以下只是节选，请酌情套用

```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛩️ 我的机场:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: https://example.com/xxx/clash
    path: ./proxies/airport.yaml
    interval: 86400
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

proxy-groups:
  - name: 🚀 节点选择
    type: select
    proxies:
      # 根据 proxy-groups 中（下方）国家或地区的节点名称进行增删改，一一对应
      - 🇭🇰 香港节点
      - 🇯🇵 日本节点

  - name: 🪜 代理域名
    type: select
    proxies:
      # 相当于调用“🚀 节点选择”内的代理组（默认选择）
      - 🚀 节点选择
      # 直连的意思，不想科学上网了就选择这个
      - DIRECT

  # 自动选择延迟最低的香港节点
  - name: 🇭🇰 香港节点
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    use:
      - 🛩️ 我的机场
    # 筛选出“香港”节点，支持正则表达式
    filter: "香港"

  # 手动选择日本节点
  - name: 🇯🇵 日本节点
    type: select
    use:
      - 🛩️ 我的机场
    filter: "日本"

rule-providers:
  # 需要代理的域名
  proxy:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/proxy.txt"
    path: ./ruleset/proxy.yaml
    interval: 86400

rules:
  # 让需要代理的域名选择“🪜 代理域名”，就可以访问了
  - RULE-SET,proxy,🪜 代理域名
```
# 四、 生成.yaml 文件链接
## 1. 生成链接
编辑完成后，点击右下角的“Create secret gist”按钮，然后点击右上角的“Raw”按钮
![QQ截图20230217171809](https://user-images.githubusercontent.com/45238096/219603714-534fe617-35b2-4f5d-acea-b2e691c50bed.png)
## 2. 修改链接
取出地址栏中的网址，添加前缀“https://gh.api.99988866.xyz/ ”并删除后面的一串随机码，**完成后该.yaml 文件直链才是最终生成的订阅链接**，该订阅链接地址不会改变，在不更改文件名的情况下即使编辑该.yaml 文件并提交了 n 次也不会改变  
举例，这是原地址：  
`https://gist.githubusercontent.com/DustinWin/a6d67d1c2c5da5ece004efcd791e4bf4/raw/df770aae2001b2eab426a385ea10bbbb35a35c52/template_whitelist.yaml`  
添加前缀“https://gh.api.99988866.xyz/ ”并删除后面的一串随机码（为当前编辑该文件生成的随机码“df770aae2001b2eab426a385ea10bbbb35a35c52”）  
![QQ截图20230316002904](https://user-images.githubusercontent.com/45238096/225376444-a476e09f-2c2d-496d-8319-a8afa2c382f4.png)  
修改后变成：  
`https://gh.api.99988866.xyz/https://gist.githubusercontent.com/DustinWin/a6d67d1c2c5da5ece004efcd791e4bf4/raw/template_whitelist.yaml`  
# 五、 导入订阅链接
## 1. 在 ShellClash 中导入订阅链接  
进入 ShellClash 配置脚本，选择 6-2，直接粘贴最终生成的订阅链接即可
## 2. 在各个平台的 Clash 客户端中导入订阅链接  
粘贴最终生成的订阅链接，直接下载即可
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
# 七、 私人定制
到了这里，相信你对里面的机制已经有了一定的认识，那么我们可以对自己的需求进行定制了  
最常见的有：我购买的机场支持奈飞和亚马逊，但仅新加坡这一个节点支持亚马逊，日本和韩国节点支持奈飞，这个规则怎么写？  
首先我们需要通过[分流规则](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash)找到奈飞和亚马逊的所有域名和 IP 段，然后开始编写：
- 注：以下只是节选，请酌情套用

```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛩️ 我的机场:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: https://example.com/xxx/clash
    path: ./proxies/airport.yaml
    interval: 86400
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

proxy-groups:
  # 打开奈飞后手动选择日本或韩国节点
  - name: 🎥 奈飞节点
    type: select
    use:
      # 使用 proxy-providers 中的节点名称
      - 🛩️ 我的机场
    # 筛选出日本和韩国节点
    filter: "日本|韩国"

  # 打开亚马逊后自动选择延迟最低的新加坡节点
  - name: 🎞️ 亚马逊节点
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    use:
      # 使用 proxy-providers 中的节点名称
      - 🛩️ 我的机场
    # 筛选出新加坡节点
    filter: "新加坡"

rule-providers:
  # 奈飞所有域名和 IP 段
  netflix:
    type: http
    behavior: classical
    # 奈飞的分流规则下载地址
    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Netflix/Netflix.yaml"
    path: ./ruleset/netflix.yaml
    interval: 86400

  # 亚马逊所有域名
  amazonprimevideo:
    type: http
    behavior: classical
    # 亚马逊的分流规则下载地址
    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/AmazonPrimeVideo/AmazonPrimeVideo.yaml"
    path: ./ruleset/amazonprimevideo.yaml
    interval: 86400

rules:
  # 两条都写在 rules 最前面
  - RULE-SET,netflix,🎥 奈飞节点
  - RULE-SET,amazonprimevideo,🎞️ 亚马逊节点
```
# 给作者加鸡腿：
## 支付宝  
![167673823486183](https://user-images.githubusercontent.com/45238096/219877760-b385af34-ebbd-438e-a31f-cd2b985047bb.png)
# 机场推荐（薯条 CNIX，自用，感觉还不错）：  
## [邀请链接 1](https://increasingthroughputhpcandaiworkloads.analytics-networking-security-storage-highperformance-computing.inspur-microsoft-azure-nvidia-oracle-ovhcloud-redhat-supermicro.aws-cisco-delltechnologies-fujitsu-hewlettpackardenterprise-ibm.com/auth/register?code=kHBf)  
## [邀请链接 2](https://increasingthroughputhpcandaiworkloads.analytics-networking-security-storage-highperformance-computing.inspur-microsoft-azure-nvidia-oracle-ovhcloud-redhat-supermicro.aws-cisco-delltechnologies-fujitsu-hewlettpackardenterprise-ibm.com/#/auth/register?code=kHBf)
