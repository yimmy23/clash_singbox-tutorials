# 分享自己使用 [ShellClash](https://github.com/juewuy/ShellClash) 搭配 geo 方案的一套配置
# 声明：
1. 此方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb）[路由规则文件](https://github.com/DustinWin/clash-geosite)，**属高度定制，仅供参考**
2. 规则参考 [DustinWin/clash-geosite](https://github.com/DustinWin/clash-geosite)
3. 请根据自身情况进行修改，**适合自己的方案才是最好的方案**，如无特殊需求，可以照搬
4. 此方案适用于 ShellClash（以 arm64 架构为例）
5. 此方案已摒弃 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)，但拦截广告效果依然强劲
---
# 一、 生成配置文件 .yaml 文件直链
具体方法此处不再赘述，请看《[生成带有自定义规则和代理组的配置文件 yaml 直链 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geo%20%E6%96%B9%E6%A1%88.md)》，贴一下我使用的配置：
- 注：`rules` 部分的 `geosite` 和 `geoip` 内容须与 `geox-url` 中的路由规则文件相匹配

```
proxy-providers:
  🛫 我的机场:
    type: http
    # 修改为你的 Clash 订阅链接
    url: "https://example.com/xxx/xxx&flag=clash"
    path: ./proxies/airport.yaml
    interval: 43200
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      url: "https://www.gstatic.com/generate_204"
      interval: 600

proxy-groups:
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

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

  # 采用节点负载均衡策略，优点是更稳定，速度可能有提升；推荐在节点复用比较多的情况下使用
  - {name: 🇭🇰 香港节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "美国"}

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
  - GEOIP,telegramip,✈️ Telegram IP
  - GEOIP,lanip,🏠 私有网络,no-resolve
  - GEOIP,cn,🇨🇳 国内 IP
  - MATCH,🐟 漏网之鱼
```
# 二、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)
我用的 Alpha 版，目前一直很稳定  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-armv8 -L https://ghproxy.com/https://github.com/DustinWin/clash-tools/releases/download/latest/clash.meta-linux-armv8
```
# 三、 导入路由规则文件和 user.yaml
注：
- 1. **路由规则文件和 user.yaml 都属高度定制，牵一发而动全身**
- 2. geosite.dat 和 user.yaml 来源于 [DustinWin/clash-geosite](https://github.com/DustinWin/clash-geosite)
- 3. geoip.dat 来源于 [DustinWin/clash-geoip](https://github.com/DustinWin/clash-geoip)

连接 SSH 后运行如下命令：
```
curl -o $clashdir/GeoSite.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/geosite.dat
curl -o $clashdir/GeoIP.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/geoip.dat
curl -o $clashdir/Country.mmdb -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/Country.mmdb
curl -o $clashdir/yamls/user.yaml -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/fake-ip-user.yaml
```
# 四、 添加定时任务
连接 SSH 后运行 `crontab -e`，按一下 Ins 键（Insert 键），在最下方粘贴如下内容：
```
30 3 * * * curl -o /data/clash/clash -L https://ghproxy.com/https://github.com/DustinWin/clash-tools/releases/download/latest/clash.meta-linux-armv8 && chmod +x /data/clash/clash && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 3 点半更新 Clash.Meta 内核
0 4 * * * curl -o /data/clash/GeoSite.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/geosite.dat && curl -o /data/clash/GeoIP.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/geoip.dat && curl -o /data/clash/Country.mmdb -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/Country.mmdb && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 4 点更新路由规则文件
30 4 * * * curl -o /data/clash/yamls/user.yaml -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/fake-ip-user.yaml && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 4 点半更新 user.yaml
0 5 * * 1,3,5 /data/clash/start.sh updateyaml && /data/clash/start.sh restart >/dev/null 2>&1 #每周一、三、五早上 5 点更新订阅并重启 Clash 服务
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车，运行如下命令：
```
/etc/init.d/cron restart
```
# 五、 设置部分
1. 设置可参考《[ShellClash 配置 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE%20geo%20%E6%96%B9%E6%A1%88.md)》，此处只列举配置的不同之处
2. 进入主菜单->2 clash功能设置，设置如下：
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/be7debcc-2d5f-4379-9d09-3f6be133de29" width="60%"/>  

3. 进入主菜单->5 设置定时任务，查看定时任务是否添加成功
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/d60e9bc9-49b9-4b93-b911-147abb822f91" width="60%"/>  

4. 进入主菜单->6 导入配置文件->6 配置文件覆写->1 自定义端口及秘钥，设置如下：
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/feea34a4-3b25-4c3d-b814-c4bbd8186636" width="60%"/>  

5. 进入主菜单->7 clash 进阶设置->6 配置内置 DNS 服务，设置如下：
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/f7054430-a528-4669-ba6d-be6ae8613f08" width="60%"/>  

6. 进入主菜单->6 导入配置文件->2 导入 Clash 配置文件链接，粘贴第一步中生成的配置文件 .yaml 文件直链，启动 clash 服务即可
# 六、 在线 Dashboard 面板
推荐使用在线 Dashboard 面板 [metacubexd](https://github.com/metacubex/metacubexd)，访问地址：https://d.metacubex.one
1. 需要设置该网站“允许不安全内容”，以 Chrome 浏览器为例，进入设置->隐私和安全->网站设置->更多内容设置->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://d.metacubex.one`  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/3d1ed229-1d3a-4ccc-a7b4-adecc8fee8b4" width="60%"/>  

2. 首次进入 https://d.metacubex.one 需要添加“host url”，输入 `http://192.168.31.1:9090` 并点击“添加”，最后点击下方新增的 http://192.168.31.1:9090 即可访问 Dashboard 面板  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/340b3519-c98c-440b-a065-11678142f883" width="60%"/>
