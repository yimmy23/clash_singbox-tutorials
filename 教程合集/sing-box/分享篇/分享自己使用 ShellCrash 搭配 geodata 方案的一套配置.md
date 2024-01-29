# 分享自己使用 [ShellCrash](https://github.com/juewuy/ShellCrash) 搭配 geodata 方案的一套配置
# 声明：
1. 此方案此方案适用于 [sing-box](https://github.com/SagerNet/sing-box)，采用 `rule_set` 规则
2. 自定义规则参考 [DustinWin/ruleset_geodata/ruleset]（https://github.com/DustinWin/ruleset_geodata/tree/master#%E4%BA%8C-ruleset-%E8%A7%84%E5%88%99%E9%9B%86%E6%96%87%E4%BB%B6%E8%AF%B4%E6%98%8E）
3. 请根据自身情况进行修改，**适合自己的方案才是最好的方案**，如无特殊需求，可以照搬
4. 此方案适用于 ShellCrash（以 arm64 架构为例，且安装路径为 */data/ShellCrash*）
5. 此方案已摒弃 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)，但拦截广告效果依然强劲
---
# 一、 生成配置文件 .json 文件直链
具体方法此处不再赘述，请看《[生成带有自定义出站和规则的 sing-box 配置文件直链-ruleset 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md)》，贴一下我使用的配置：
```
{
  "outbounds": [
    { "tag": "🚀 节点选择", "type": "selector", "outbounds": [ "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇰🇷 韩国节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点", "🇬🇧 英国节点" ] },
    { "tag": "📈 网络测试", "type": "selector", "outbounds": [ "🎯 全球直连", "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇰🇷 韩国节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点", "🇬🇧 英国节点" ] },
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
    { "tag": "🎯 全球直连", "type": "selector", "outbounds": [ "DIRECT" ] },
    { "tag": "GLOBAL", "type": "selector", "outbounds": [ "DIRECT", "🇭🇰 香港节点", "🆓 免费节点", "🇹🇼 台湾节点", "🇯🇵 日本节点", "🇰🇷 韩国节点", "🇸🇬 新加坡节点", "🇺🇸 美国节点", "🇬🇧 英国节点" ] },
    { "tag": "DIRECT", "type": "direct", "domain_strategy": "prefer_ipv6" },
    { "tag": "dns-out", "type": "dns" },
    {
      "tag": "🆓 免费节点",
      "type": "vless",
      "server": "example.com",
      "server_port": 443,
      "uuid": "{uuid}",
      "network": "tcp",
      "tls": { "enabled": true, "server_name": "example.com", "insecure": false },
      "transport": { "type": "ws", "path": "/?ed=2048", "headers": { "Host": "example.com" } },
      "domain_strategy": "prefer_ipv6"
    },
    { "tag": "🇭🇰 香港节点", "type": "urltest", "tolerance": 100, "providers": [ "🅱️ Bitz Net" ], "includes": [ "香港.*BGP" ] },
    { "tag": "🇹🇼 台湾节点", "type": "urltest", "tolerance": 100, "providers": [ "🅱️ Bitz Net" ], "includes": [ "台湾" ] },
    { "tag": "🇯🇵 日本节点", "type": "urltest", "tolerance": 100, "providers": [ "🅱️ Bitz Net" ], "includes": [ "日本" ] },
    { "tag": "🇰🇷 韩国节点", "type": "urltest", "tolerance": 100, "providers": [ "🅱️ Bitz Net" ], "includes": [ "韩国" ] },
    { "tag": "🇸🇬 新加坡节点", "type": "urltest", "tolerance": 100, "providers": [ "🅱️ Bitz Net" ], "includes": [ "新加坡" ] },
    { "tag": "🇺🇸 美国节点", "type": "urltest", "tolerance": 100, "providers": [ "🅱️ Bitz Net" ], "includes": [ "美国" ] },
    { "tag": "🇬🇧 英国节点", "type": "urltest", "tolerance": 100, "providers": [ "🅱️ Bitz Net" ], "includes": [ "英国" ] }
  ],
  "route": {
    "rules": [
      { "protocol": "dns", "outbound": "dns-out" },
      { "clash_mode": "Global", "outbound": "GLOBAL" },
      { "clash_mode": "Direct", "outbound": "DIRECT" },
      { "rule_set": "private", "outbound": "🔒 私有网络" },
      { "rule_set": "microsoft-cn", "outbound": "Ⓜ️ 微软服务" },
      { "rule_set": "apple-cn", "outbound": "🍎 苹果服务" },
      { "rule_set": "google-cn", "outbound": "📢 谷歌服务" },
      { "rule_set": "games-cn", "outbound": "🎮 游戏平台" },
      { "rule_set": "networktest", "outbound": "📈 网络测试" },
      { "rule_set": "proxy", "outbound": "🪜 代理域名" },
      { "rule_set": "cn", "outbound": "🔗 直连域名" },
      { "rule_set": "telegramip", "outbound": "📲 电报消息" },
      { "rule_set": "privateip", "outbound": "🔒 私有网络" },
      { "rule_set": "cnip", "outbound": "🇨🇳 国内 IP" }
    ],
    "rule_set": [
      {
        "tag": "private",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/private.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "microsoft-cn",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/microsoft-cn.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "apple-cn",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/apple-cn.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "google-cn",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/google-cn.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "games-cn",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/games-cn.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "networktest",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/networktest.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "proxy",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/proxy.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "cn",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/cn.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "telegramip",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/telegramip.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "privateip",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/privateip.srs",
        "download_detour": "DIRECT"
      },
      {
        "tag": "cnip",
        "type": "remote",
        "format": "binary",
        "url": "https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/cnip.srs",
        "download_detour": "DIRECT"
      }
    ],
    "final": "🐟 漏网之鱼"
  }
}
```
# 二、 导入 [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-armv8 -L https://fastly.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-puernya/sing-box-linux-armv8 && crash
```
此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 4 Sing-Box 内核
# 三、 编辑 outbound_providers.json
连接 SSH 后执行 `vi $CRASHDIR/jsons/outbound_providers.json`，按一下 Ins 键（Insert 键），编辑如下内容并粘贴：
```
{
  "outbound_providers": [
    {
      "tag": "🅱️ Bitz Net",
      "type": "http",
      "healthcheck_url": "https://www.gstatic.com/generate_204",
      "healthcheck_interval": "10m",
      # # 修改为你的 Clash 订阅链接
      "download_url": "https://example.com/xxx/xxx&flag=clash",
      "path": "./yamls/airport.yaml",
      "download_ua": "clash.meta",
      "download_interval": "24h",
      "download_detour": "DIRECT",
      "override_dialer": { "domain_strategy": "prefer_ipv6" }
    }
  ]
}
```
四、 导入 dns.json
连接 SSH 后运行如下命令：
```
curl -o $CRASHDIR/jsons/dns.json -L https://fastly.jsdelivr.net/gh/DustinWin/ruleset_geodata@sing-box/ruleset-dns.json
```
# 五、 添加定时任务
1. 连接 SSH 后运行 `vi $CRASHDIR/task/task.user`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
201#curl -o /data/ShellCrash/CrashCore -L https://fastly.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-puernya/sing-box-linux-armv8 && chmod +x /data/ShellCrash/CrashCore && /data/ShellCrash/start.sh restart >/dev/null 2>&1#更新sing-box_PuerNya版内核
```
2. 按一下 Esc 键（退出键），输入英文冒号`:`，继续输入 `wq` 并回车
3. 执行 `crash`，进入 ShellCrash->5 配置自动任务->1 添加自动任务，可以看到末尾就有添加的定时任务，输入对应的数字并回车后可设置执行条件  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/3f65431a-bdff-48d3-addb-82e6888fbfaa" width="60%"/>

# 六、 设置部分
1. 设置可参考《[ShellCrash 配置-ruleset 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md)》，此处只列举配置的不同之处
2. 进入主菜单->6 导入配置文件->6 配置文件覆写->1 自定义端口及秘钥，设置如下：  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/feea34a4-3b25-4c3d-b814-c4bbd8186636" width="60%"/>

3. 进入主菜单->7 内核进阶设置->6 配置内置 DNS 服务，设置如下：
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/2ed13f2b-625b-4673-8bc6-f92552ea3c66" width="60%"/>

4. 进入主菜单->6 导入配置文件->2 导入外部配置文件链接，粘贴第《一》步中生成的配置文件 .json 文件直链，启动服务即可
# 七、 在线 Dashboard 面板
推荐使用在线 Dashboard 面板 [metacubexd](https://github.com/metacubex/metacubexd)，访问地址：https://d.metacubex.one
1. 需要设置该网站“允许不安全内容”，以 Chrome 浏览器为例，进入设置->隐私和安全->网站设置->更多内容设置->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://d.metacubex.one`  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/3d1ed229-1d3a-4ccc-a7b4-adecc8fee8b4" width="60%"/>

2. 首次进入 https://d.metacubex.one 需要添加“后端地址”，输入 `http://192.168.31.1:9090` 并点击“添加”即可访问 Dashboard 面板  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/56eaeb14-f5e3-4245-b04b-680be7f01b3e" width="60%"/>
