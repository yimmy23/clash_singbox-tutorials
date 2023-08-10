# [Clash Verge](https://github.com/zzzgydi/clash-verge)（Windows 端）配置
- 注：此方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） [路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
# 一、 设置部分
1. 进入 Clash Verge->设置->Verge 设置->语言设置，可切换到“中文”
2. 进入设置->Clash 设置->Clash 内核，点击“螺帽图标”并切换至“[Clash Meta 内核](https://github.com/MetaCubeX/Clash.Meta)”
3. 进入设置->系统设置->服务模式，点击“盾牌图标”，“INSTALL”即可
# 一、 导入或更新 Clash Meta 内核
编辑文本文档，粘贴如下内容：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %PROGRAMFILES%\Clash Verge\clash-meta.exe -L https://ghproxy.com/https://github.com/DustinWin/clash-tools/raw/main/Clash.Meta-release/clash.meta-windows-amd64.exe
pause
```
# 二、 导入路由规则集文件
编辑文本文档，粘贴如下内容：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im clash-meta*
curl -o %USERPROFILE%\.config\clash-verge\geosite.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat
curl -o %USERPROFILE%\.config\clash-verge\geoip.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat
curl -o %USERPROFILE%\.config\clash-verge\Country.mmdb -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb
copy /y "%USERPROFILE%\.config\clash-verge\geosite.dat" "%PROGRAMFILES%\Clash Verge\resources"
copy /y "%USERPROFILE%\.config\clash-verge\geoip.dat" "%PROGRAMFILES%\Clash Verge\resources"
copy /y "%USERPROFILE%\.config\clash-verge\Country.mmdb" "%PROGRAMFILES%\Clash Verge\resources"
pause
```
另存为 .bat 文件，右击并选择以管理员身份运行
# 三、 导入配置
## 1. 导入配置文件
① 进入 Clash Verge->配置，在“配置文件链接”处粘贴《[生成带有自定义规则和代理组的配置文件 yaml 直链 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geo%20%E6%96%B9%E6%A1%88.md)》中生成的配置文件 .yaml 文件直链  
- 注：如有 BT 下载需求，为了过滤 P2P 流量，建议在 `rules` 靠前添加`- DST-PORT,6881-6889,🎯 全球直连`
如：
```
rules:
  - GEOSITE,ads,⛔️ 广告域名
  - DST-PORT,6881-6889,🎯 全球直连
  - GEOSITE,lan,🏠 私有网络
  - ...
```
② 右击导入的配置文件，选择“编辑信息”，“更新间隔”设置为“1440”，然后“保存”
## 2. 新建自定义配置
① 进入 Clash Verge->配置，点击“新建”，类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”  
② 再次右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
mode: rule
ipv6: true
log-level: silent
allow-lan: true
mixed-port: 7890
unified-delay: false
tcp-concurrent: true
external-controller: 127.0.0.1:9090

find-process-mode: strict
global-client-fingerprint: chrome

tun:
  enable: true
  stack: system
  dns-hijack:
    - 'any:53'
  auto-route: true
  auto-detect-interface: true

profile:
  store-selected: true
  store-fake-ip: true

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  use-hosts: true
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter:
    - '*.lan'
    - '*.localdomain'
    - '*.example'
    - '*.invalid'
    - '*.localhost'
    - '*.test'
    - '*.local'
    - '*.home.arpa'
    - 'time.*.com'
    - 'time.*.gov'
    - 'time.*.edu.cn'
    - 'time.*.apple.com'
    - 'time1.*.com'
    - 'time2.*.com'
    - 'time3.*.com'
    - 'time4.*.com'
    - 'time5.*.com'
    - 'time6.*.com'
    - 'time7.*.com'
    - 'ntp.*.com'
    - 'ntp1.*.com'
    - 'ntp2.*.com'
    - 'ntp3.*.com'
    - 'ntp4.*.com'
    - 'ntp5.*.com'
    - 'ntp6.*.com'
    - 'ntp7.*.com'
    - '*.time.edu.cn'
    - '*.ntp.org.cn'
    - '+.pool.ntp.org'
    - '*.music.163.com'
    - '*.126.net'
    - '*.kuwo.cn'
    - '*.y.qq.com'
    - '*.xiami.com'
    - '*.music.migu.cn'
    - '+.msftconnecttest.com'
    - '+.msftncsi.com'
    - '+.qq.com'
    - '+.tencent.com'
    - '+.srv.nintendo.net'
    - '*.n.n.srv.nintendo.net'
    - '+.stun.playstation.net'
    - 'xbox.*.*.microsoft.com'
    - '*.*.xboxlive.com'
    - 'xbox.*.microsoft.com'
    - '+.battlenet.com.cn'
    - '+.wotgame.cn'
    - '+.wggames.cn'
    - '+.wowsgame.cn'
    - '+.wargaming.net'
    - 'stun.*.*'
    - 'stun.*.*.*'
    - '+.stun.*.*'
    - '+.stun.*.*.*'
    - '+.stun.*.*.*.*'
    - '+.stun.*.*.*.*.*'
    - '*.linksys.com'
    - '*.linksyssmartwifi.com'
    - '*.router.asus.com'
    - '+.nflxvideo.net'
    - '*.square-enix.com'
    - '*.finalfantasyxiv.com'
    - '*.ffxiv.com'
    - '*.ff14.sdo.com'
    - '*.mcdn.bilivideo.cn'
    - '+.media.dssott.com'
    - '+.cmbchina.com'
    - '+.cmbimg.com'
    - '+.sandai.net'
    - '+.n0808.com'
    - time-ios.apple.com
    - time1.cloud.tencent.com
    - music.163.com
    - musicapi.taihe.com
    - music.taihe.com
    - songsearch.kugou.com
    - trackercdn.kugou.com
    - api-jooxtt.sanook.com
    - api.joox.com
    - joox.com
    - y.qq.com
    - streamoc.music.tc.qq.com
    - mobileoc.music.tc.qq.com
    - isure.stream.qqmusic.qq.com
    - dl.stream.qqmusic.qq.com
    - aqqmusic.tc.qq.com
    - amobile.music.tc.qq.com
    - music.migu.cn
    - localhost.ptlogin2.qq.com
    - localhost.sec.qq.com
    - xnotify.xboxlive.com
    - proxy.golang.org
    - heartbeat.belkin.com
    - mesu.apple.com
    - swscan.apple.com
    - swquery.apple.com
    - swdownload.apple.com
    - swcdn.apple.com
    - swdist.apple.com
    - lens.l.google.com
    - stun.l.google.com
    - na.b.g-tun.com
    - ff.dorado.sdo.com
    - shark007.net
    - local.adguard.org
  default-nameserver:
    - https://1.12.12.12/dns-query
    - https://223.5.5.5/dns-query
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
```
# 四、 启动 Clash
1. 进入 Clash Verge->设置->Clash 设置->Clash 字段，勾选带有感叹号的字段，“保存”即可
2. 进入设置->系统设置->Tun 模式，启用
# 五、 在线面板
推荐使用在线面板 [Yacd-meta](https://github.com/MetaCubeX/Yacd-meta)，访问地址：https://yacd.metacubex.one  
1. 需要设置该网站“允许不安全内容”，以 [Chrome 浏览器](https://www.google.com/chrome)为例，进入设置-->隐私和安全-->网站设置-->更多内容设置-->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://yacd.metacubex.one`
<img src="https://user-images.githubusercontent.com/45238096/235448980-52331db5-6b9f-4b0c-a876-1509d34db51a.png" width="60%"/>  

2. 首次进入 https://yacd.metacubex.one 需要添加“API Base URL”，输入 `http://192.168.31.1:9090` 并点击“Add”，最后点击下方新增的 http://192.168.31.1:9090 即可访问 Dashboard 面板
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/086aa876-109a-4a6f-9e9b-22269a869b4f" width="60%"/>
