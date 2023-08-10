# [ShellClash](https://github.com/juewuy/ShellClash) 本地配置自定义规则和代理组 ruleset 方案
- 注：此方案采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
# 前言
1. 本教程只适用于 ShellClash
2. 自定义规则参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
3. 本教程**仅适合白名单模式**（没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户），黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）慎用
4. 本教程最终效果媲美《[生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20ruleset%20%E6%96%B9%E6%A1%88.md)》（代理组更直观，操作更方便），但不依赖于网络
5. 所有步骤完成后，请连接 SSH 执行命令 `$clashdir/start.sh restart` 后生效
---
# 一、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)和路由规则文件
可参考《[ShellClash 配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE.md)》里的步骤《一、二》进行操作
# 二、 导入配置文件
1. 进入 ShellClash->6 导入配置文件->1 在线生成Clash配置文件->4 选取在线配置规则模版，选择 4 [Acl4SSR](https://acl4ssr-sub.github.io) 极简版（适合自建节点）  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/88b58a87-76b8-4004-b005-133d6a2bb71f" width="60%"/>

2. 进入 ShellClash->6 导入配置文件->1 在线生成Clash配置文件，输入订阅链接后回车，再输入“1”并回车即可
# 三、 自定义规则和代理组
## 1. 自定义 others.yaml
连接 SSH 后执行命令 `vi $clashdir/yamls/others.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
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

# 规则集 .yaml 文件；每天自动更新
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
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 2. 自定义 proxy-groups.yaml
连接 SSH 后执行命令`vi $clashdir/yamls/proxy-groups.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
# 手动选择国家或地区节点；根据 proxy-groups 中（下方）国家或地区的节点名称对 proxies 值进行增删改，须一一对应
- name: 🈯 节点指定
  type: select
  proxies:
    - 🇭🇰 香港节点
    - 🇹🇼 台湾节点
    - 🇯🇵 日本节点
    - 🇰🇷 韩国节点
    - 🇸🇬 新加坡节点
    - 🇺🇸 美国节点

# Speedtest 测速网站：选择“全球直连”为测试本地网络速度（运营商网络速度），可选择其它节点用于测试机场节点速度
- name: 📈 网络测试
  type: select
  proxies:
    - 🎯 全球直连
    - 🇭🇰 香港节点
    - 🇹🇼 台湾节点
    - 🇯🇵 日本节点
    - 🇰🇷 韩国节点
    - 🇸🇬 新加坡节点
    - 🇺🇸 美国节点

- name: ⚡ 直连域名
  type: select
  proxies:
    - 🎯 全球直连
    - 🈯 节点指定

- name: 🪜 代理域名
  type: select
  proxies:
    - 🈯 节点指定
    - 🎯 全球直连

- name: 🎮 国区游戏
  type: select
  proxies:
    - 🎯 全球直连
    - 🈯 节点指定

- name: Ⓜ️ Microsoft 中国
  type: select
  proxies:
    - 🎯 全球直连
    - 🈯 节点指定

- name: 🗽 Google 中国
  type: select
  proxies:
    - 🎯 全球直连
    - 🈯 节点指定

- name: 🍎 Apple 中国
  type: select
  proxies:
    - 🎯 全球直连
    - 🈯 节点指定

- name: 🏠 私有网络
  type: select
  proxies:
    - 🎯 全球直连

- name: ⛔️ 广告域名
  type: select
  proxies:
    - 🛑 全球拦截

- name: 🛑 全球拦截
  type: select
  proxies:
    - REJECT

# -----------------国家或地区节点----------------------

# 自动选择节点，即按照 url 测试结果使用延迟最低的节点
- name: 🇭🇰 香港节点
  type: url-test
  # 测试后容差大于 100ms 才会切换到延迟低的那个节点
  tolerance: 100
  # 未选择到当前策略组时不会进行延迟测试
  lazy: true
  use:
    - 🛫 我的机场 1
    - 🛫 我的机场 2
  # 筛选出“香港”节点，支持正则表达式
  filter: "香港"

- name: 🇹🇼 台湾节点
  type: url-test
  tolerance: 100
  lazy: true
  use:
    - 🛫 我的机场 1
    - 🛫 我的机场 2
  filter: "台湾"

- name: 🇯🇵 日本节点
  type: url-test
  tolerance: 100
  lazy: true
  use:
    - 🛫 我的机场 1
    - 🛫 我的机场 2
  filter: "日本"

- name: 🇰🇷 韩国节点
  type: url-test
  tolerance: 100
  lazy: true
  use:
    - 🛫 我的机场 1
    - 🛫 我的机场 2
  filter: "韩国"

- name: 🇸🇬 新加坡节点
  type: url-test
  tolerance: 100
  lazy: true
  use:
    - 🛫 我的机场 1
    - 🛫 我的机场 2
  filter: "新加坡"

- name: 🇺🇸 美国节点
  type: url-test
  tolerance: 100
  lazy: true
  use:
    - 🛫 我的机场 1
    - 🛫 我的机场 2
  filter: "美国"
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 3. 自定义 rules.yaml
连接 SSH 后执行命令`vi $clashdir/yamls/rules.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
# 自定义规则优先放前面
- RULE-SET,reject,⛔️ 广告域名
- RULE-SET,lan,🏠 私有网络
- RULE-SET,networktest,📈 网络测速
- RULE-SET,microsoft-cn,Ⓜ️ Microsoft 中国
- RULE-SET,apple-cn,🍎 Apple 中国
- RULE-SET,google-cn,🗽 Google 中国
- RULE-SET,games-cn,🎮 国区游戏
- RULE-SET,proxy,🪜 代理域名
- RULE-SET,direct,⚡ 直连域名
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
# 四、 修改规则或代理组
**举例：我想添加一个规则，使奈飞走日本和新加坡节点**  
① 进入 [blackmatrix7/ios_rule_script/rule/Clash](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash) 后按 Ctrl+F 组合键分别搜索“netflix”  
② 可以精确搜索到“netflix”
③ 进入指定目录并点击 yaml 文件，然后点击“Raw”获取下载地址，并将下载地址[转换为 CDN 链接](https://www.jsdelivr.com/github)，那么就可以这样编写：
注：
- 1. **一定要保证缩进对齐！一定要保证缩进对齐！一定要保证缩进对齐！**
- 2. 以下只是节选，请酌情套用
- 3. 推荐使用 [VSCode 编辑器](https://code.visualstudio.com/Download) 或其它专业文本编辑器

## 1. 修改 others.yaml 文件
① 连接 SSH 后执行命令 `vi $clashdir/yamls/others.yaml`，按一下 Ins 键（Insert 键），在 `rule-providers` 内粘贴如下内容：
```
  netflix:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Netflix/Netflix.yaml'
    path: ./ruleset/netflix.yaml
    interval: 86400
```
② 按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 2. 修改 proxy-groups.yaml 文件
连接 SSH 后执行命令 `vi $clashdir/yamls/proxy-groups.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
# 打开奈飞后自动选择延迟最低的日本或新加坡节点；容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试
- name: 🎥 奈飞节点
  type: url-test
  tolerance: 100
  use:
    - 🛫 我的机场 1
    - 🛫 我的机场 2
  filter: "日本|新加坡"
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 3. 修改 rules.yaml 文件
连接 SSH 后执行命令 `vi $clashdir/yamls/rules.yaml`，按一下 Ins 键（Insert 键），**优先在最上方**粘贴如下内容：
```
# 自定义规则优先放前面
- RULE-SET,netflix,🎥 奈飞节点
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
# 五、 添加小规则
仅添加特定网址走直连或走代理，连接 SSH 后执行命令 `vi $clashdir/yamls/rules.yaml`，按一下 Ins 键（Insert 键），在**最上方**粘贴如下内容：  
注：
- 1. 以下内容只是举例，请根据自身需要进行增删改
- 2. 其它规则请参考《[Clash.Meta Wiki](https://wiki.metacubex.one/config/rules)》

```
# 以 googleapis.cn 为后缀的所有域名走代理
- DOMAIN-SUFFIX,googleapis.cn,🚀 节点选择

# 与哔哩哔哩相关的所有域名走直连
- GEOSITE,bilibili,DIRECT

# 含有 ipv6 关键字的所有域名走直连
- DOMAIN-KEYWORD,ipv6,DIRECT
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
