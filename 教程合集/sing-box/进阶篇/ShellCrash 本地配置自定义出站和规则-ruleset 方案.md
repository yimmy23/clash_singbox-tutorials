# [ShellCrash](https://github.com/juewuy/ShellCrash) 本地配置自定义出站和规则-ruleset 方案
- 注：此方案适用于 [sing-box](https://github.com/SagerNet/sing-box)，采用 `rule_set` 规则
# 前言：
1. 本教程只适用于 ShellCrash
2. 自定义规则参考 [DustinWin/ruleset_geodata/ruleset](https://github.com/DustinWin/ruleset_geodata#%E4%BA%8C-ruleset-%E8%A7%84%E5%88%99%E9%9B%86%E6%96%87%E4%BB%B6%E8%AF%B4%E6%98%8E)
3. 本教程**仅适合白名单模式**（没有命中规则的网络流量统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）
4. 本教程最终效果媲美《[生成带有自定义出站和规则的 sing-box 配置文件直链-ruleset 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md)》（出站分组更直观，操作更方便），但不依赖于网络
5. 若仅配置自定义出站和规则，可直接跳过第《二》步
6. 代理集合 outbound_providers.json、出站 outbounds.json 和规则 route.json 为合并模式（在基础配置上新增）
7. 所有步骤完成后，请连接 SSH 执行命令 `$CRASHDIR/start.sh restart` 后生效
8.  推荐使用 [Visual Studio Code](https://code.visualstudio.com/Download) 等专业编辑器来修改配置文件
---
# 一、 导入 [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)
可参考《[ShellCrash 配置-ruleset 方案/导入 sing-box PuerNya 版内核](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md#%E4%B8%80-%E5%AF%BC%E5%85%A5-sing-box-puernya-%E7%89%88%E5%86%85%E6%A0%B8)》里的步骤进行操作
# 二、 导入配置文件
1. 进入 ShellCrash -> 6 导入配置文件 -> 1 在线生成 singbox 配置文件 -> 4 选取在线配置规则模版，选择 4 [ACL4SSR](https://acl4ssr-sub.github.io) 极简版（适合自建节点）  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/70e60a4d-6819-43aa-9f6c-0a0ab4384db6" width="60%"/>

2. 进入 ShellCrash -> 6 导入配置文件 -> 1 在线生成 singbox 配置文件，输入订阅链接后回车，再输入“1”并回车即可
# 三、 自定义出站和规则
## 1. 自定义代理集合 outbound_providers.json（用于添加自定义出站提供者）
<details>
<summary>展开/收起</summary>

① 连接 SSH 后执行命令 `mkdir -p $CRASHDIR/providers/`
- 注：因 `outbound_providers` 代理集合配置的 `path` 路径中含有文件夹“*providers*”，须手动新建此文件夹才能使 .yaml 订阅文件保存到本地，否则将保存到内存中（每次启动服务都要重新下载）

② 继续执行命令 `vi $CRASHDIR/jsons/outbound_providers.json`，按一下 Ins 键（Insert 键），编辑如下内容并粘贴：
```
{
  // 代理集合（获取机场订阅链接内的所有节点）
  "outbound_providers": [
    {
      "tag": "🛫 我的机场 1",
      "type": "remote",
      // 机场订阅链接，使用 Clash 链接
      "download_url": "https://example.com/xxx/xxx&flag=clash",
      "path": "./providers/airport1.yaml",
      "download_interval": "24h",
      "download_ua": "clash.meta",
      // 初步筛选需要的节点，可有效减轻路由器压力，支持正则表达式，若不筛选可删除此配置项
      "includes": [ "香港|台湾|日本|新加坡|美国" ],
      // 初步排除不需要的节点，支持正则表达式，若不排除可删除此配置项
      "excludes": "高倍|×10",
      "healthcheck_url": "https://www.gstatic.com/generate_204",
      "healthcheck_interval": "10m"
    },
    {
      "tag": "🛫 我的机场 2",
      "type": "remote",
      "download_url": "https://example.com/xxx/xxx&flag=clash",
      "path": "./providers/airport2.yaml",
      "download_interval": "24h",
      "download_ua": "clash.meta",
      "includes": [ "香港|台湾|日本|新加坡|美国" ],
      "excludes": "高倍|×10",
      "healthcheck_url": "https://www.gstatic.com/generate_204",
      "healthcheck_interval": "10m"
    }
  ]
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
</details>

## 2. 自定义出站 outbounds.json（用于添加自定义出站）
<details>
<summary>展开/收起</summary>

连接 SSH 后执行命令 `vi $CRASHDIR/jsons/outbounds.json`，按一下 Ins 键（Insert 键），编辑如下内容并粘贴：
```
{
  // 出站
  "outbounds": [
    // 手动选择国家或地区节点；根据“国家或地区出站”的名称对 `outbounds` 值进行增删改，须一一对应
    { "tag": "🈯 节点指定", "type": "selector", "outbounds": [ "🇭🇰 香港节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点", "🆓 免费节点" ] },
    // 选择`🎯 全球直连`为测试本地网络（运营商网络速度和 IPv6 支持情况），可选择其它节点用于测试机场节点速度和 IPv6 支持情况
    { "tag": "📈 网络测试", "type": "selector", "outbounds": [ "🎯 全球直连", "🇭🇰 香港节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点", "🆓 免费节点" ] },
    { "tag": "🤖 人工智能", "type": "selector", "outbounds": [ "🇭🇰 香港节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇰🇷 韩国节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点" ] },
    { "tag": "🎮 游戏服务", "type": "selector", "outbounds": [ "🎯 全球直连", "🈯 节点指定" ] },
    { "tag": "🪟 微软服务", "type": "selector", "outbounds": [ "🎯 全球直连", "🈯 节点指定" ] },
    { "tag": "🇬 谷歌服务", "type": "selector", "outbounds": [ "🎯 全球直连", "🈯 节点指定" ] },
    { "tag": "🍎 苹果服务", "type": "selector", "outbounds": [ "🎯 全球直连", "🈯 节点指定" ] },
    { "tag": "🇨🇳 直连域名", "type": "selector", "outbounds": [ "🎯 全球直连", "🈯 节点指定" ] },
    { "tag": "🇨🇳 直连 IP", "type": "selector", "outbounds": [ "🎯 全球直连", "🈯 节点指定" ] },
    { "tag": "🪜 代理域名", "type": "selector", "outbounds": [ "🈯 节点指定", "🎯 全球直连" ] },
    { "tag": "📲 电报消息", "type": "selector", "outbounds": [ "🈯 节点指定" ] },
    { "tag": "🔒 私有网络", "type": "selector", "outbounds": [ "🎯 全球直连" ] },
    { "tag": "🛑 广告拦截", "type": "selector", "outbounds": [ "REJECT" ] },

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
    // 自动选择节点，即按照 url 测试结果使用延迟最低的节点；测试后容差大于 50ms 才会切换到延迟低的那个节点；筛选出“香港”节点，支持正则表达式
    { "tag": "🇭🇰 香港节点", "type": "urltest", "tolerance": 50, "use_all_providers": true, "includes": [ "(?i)港|hk|hongkong|hong kong" ] },
    { "tag": "🇹🇼 台湾节点", "type": "urltest", "tolerance": 50, "use_all_providers": true, "includes": [ "(?i)台|tw|taiwan" ] },
    { "tag": "🇯🇵 日本节点", "type": "urltest", "tolerance": 50, "use_all_providers": true, "includes": [ "(?i)日本|jp|japan" ] },
    { "tag": "🇸🇬 新加坡节点", "type": "urltest", "tolerance": 50, "use_all_providers": true, "includes": [ "(?i)新|sg|singapore" ] },
    { "tag": "🇺🇸 美国节点", "type": "urltest", "tolerance": 50, "use_all_providers": true, "includes": [ "(?i)美|us|unitedstates|united states" ] }
  ]
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
</details>

## 3. 自定义规则 route.json（用于添加自定义路由和规则）
<details>
<summary>展开/收起</summary>

① 连接 SSH 后执行命令 `mkdir -p $CRASHDIR/ruleset/`
- 注：因 `route.rule_set` 代理集合配置的 `path` 路径中含有文件夹“*ruleset*”，须手动新建此文件夹才能使 .srs 规则集文件保存到本地，否则将保存到内存中（每次启动服务都要重新下载）

② 继续执行命令 `vi $CRASHDIR/jsons/route.json`，按一下 Ins 键（Insert 键），编辑如下内容并粘贴：
```
{
  // 路由
  "route": {
    // 规则
    "rules": [
      // 自定义规则优先放前面
      { "rule_set": [ "ads" ], "outbound": "🛑 广告拦截" },
      { "rule_set": [ "private" ], "outbound": "🔒 私有网络" },
      { "rule_set": [ "microsoft-cn" ], "outbound": "🪟 微软服务" },
      { "rule_set": [ "apple-cn" ], "outbound": "🍎 苹果服务" },
      { "rule_set": [ "google-cn" ], "outbound": "🇬 谷歌服务" },
      { "rule_set": [ "games-cn" ], "outbound": "🎮 游戏服务" },
      { "rule_set": [ "ai" ], "outbound": "🤖 人工智能" },
      { "rule_set": [ "networktest" ], "outbound": "📈 网络测试" },
      { "rule_set": [ "proxy" ], "outbound": "🪜 代理域名" },
      { "rule_set": [ "cn" ], "outbound": "🇨🇳 直连域名" },
      { "rule_set": [ "telegramip" ], "outbound": "📲 电报消息", "skip_resolve": true },
      { "rule_set": [ "privateip" ],  "outbound": "🔒 私有网络", "skip_resolve": true },
      { "rule_set": [ "cnip" ], "outbound": "🇨🇳 直连 IP" }
    ],
    // 规则集（binary 文件每天自动更新）
    "rule_set": [
      {
        "tag": "ads",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/ads.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/ads.srs"
      },
      {
        "tag": "private",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/private.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/private.srs"
      },
      {
        "tag": "microsoft-cn",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/microsoft-cn.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/microsoft-cn.srs"
      },
      {
        "tag": "apple-cn",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/apple-cn.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/apple-cn.srs"
      },
      {
        "tag": "google-cn",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/google-cn.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/google-cn.srs"
      },
      {
        "tag": "games-cn",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/games-cn.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/games-cn.srs"
      },
      {
        "tag": "ai",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/ai.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/ai.srs"
      },
      {
        "tag": "networktest",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/networktest.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/networktest.srs"
      },
      {
        "tag": "proxy",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/proxy.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/proxy.srs"
      },
      {
        "tag": "cn",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/cn.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/cn.srs"
      },
      {
        "tag": "telegramip",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/telegramip.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/telegramip.srs"
      },
      {
        "tag": "privateip",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/privateip.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/privateip.srs"
      },
      {
        "tag": "cnip",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/cnip.srs",
        "url": "https://raw.githubusercontent.com/DustinWin/ruleset_geodata/sing-box-ruleset/cnip.srs"
      }
    ],
    "concurrent_dial": true
  }
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
</details>

**贴一张面板效果图（举个例子：我手动选择 `🇹🇼 台湾节点` 出站，而该出站是将机场内所有台湾节点按照 url 测试结果自动选择延迟最低的台湾节点）：**  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/e04a650c-5c88-4852-bbaa-38cc85ec5036" width="60%"/>

# 四、 修改出站或规则
**举例：我想添加一个规则，使奈飞走日本和新加坡节点**
1. 进入 [MetaCubeX/meta-rules-dat/sing/geo](https://github.com/MetaCubeX/meta-rules-dat/tree/sing/geo) 后在左侧“Go to file”搜索框内搜索“netflix”  
2. 输入“netflix”可以搜索到“geo/geosite/netflix.srs”和“geo/geoip/netflix.srs”  
3. 点击 .srs 文件，然后点击“Raw”获取下载地址，那么就可以这样编写：  
注：
- 1. **一定要保证缩进对齐！一定要保证缩进对齐！一定要保证缩进对齐！**
- 2. 以下只是节选，请酌情套用

<details>
<summary>① 修改 outbounds.json 文件</summary>

连接 SSH 后执行命令 `vi $CRASHDIR/jsons/outbounds.json`，按一下 Ins 键（Insert 键），编辑如下内容并粘贴：
```
{
  // 出站
  "outbounds": [
    // 打开奈飞后自动选择延迟最低的日本或新加坡节点；容差大于 50ms 才会切换到延迟低的那个节点
    { "tag": "🎥 奈飞视频", "type": "urltest", "tolerance": 50, "use_all_providers": true, "includes": [ "(?i)日本|jp|japan|新|sg|singapore" ] }
  ]
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
</details>
<details>
<summary>② 修改 route.json 文件</summary>

连接 SSH 后执行命令 `vi $CRASHDIR/jsons/route.json`，按一下 Ins 键（Insert 键），优先在最上方编辑如下内容并粘贴：
```
{
  // 路由
  "route": {
    // 规则
    "rules": [
      // 自定义规则优先放前面
      { "rule_set": [ "netflix" ], "outbound": "🎥 奈飞视频" }
      { "rule_set": [ "netflixip" ], "outbound": "🎥 奈飞视频", "skip_resolve": true }
    ],
    // 规则集（binary 文件每天自动更新）
    "rule_set": [
      {
        "tag": "netflix",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/netflix.srs",
        "url": "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/netflix.srs"
      },
      {
        "tag": "netflixip",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/netflixip.srs",
        "url": "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geoip/netflix.srs"
      }
    ]
  }
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
</details>

# 五、 添加小规则
仅添加特定网址走直连或走代理，连接 SSH 后执行命令 `vi $CRASHDIR/jsons/route.json`，按一下 Ins 键（Insert 键），在**最上方**粘贴如下内容：  
注：
- 1. 以下内容只是举例，请根据自身需要进行增删改
- 2. 其它规则请参考《[sing-box Wiki](https://sing-box.sagernet.org/zh/configuration/route/rule)》

```
{
  // 路由
  "route": {
    // 规则
    "rules": [
      // 以 googleapis.cn 为后缀（包括 googleapis.cn）的所有域名走代理
      { "domain_suffix": [ "googleapis.cn" ], "outbound": "🈯 节点指定" },
      // 与哔哩哔哩相关的所有域名走直连
      { "rule_set": [ "bilibili" ], "outbound": "DIRECT" },
      // 含有 ipv6 关键字的所有域名走直连
      { "domain_keyword": [ "ipv6" ], "outbound": "DIRECT" }
    ],
    // 规则集（binary 文件每天自动更新）
    "rule_set": [
      {
        "tag": "bilibili",
        "type": "remote",
        "format": "binary",
        "path": "./ruleset/bilibili.srs",
        "url": "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/bilibili.srs"
      }
    ]
  }
}
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
