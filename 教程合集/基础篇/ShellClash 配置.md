注：
- 1. 本教程中的下载链接以 CPU 架构 ARMv8 为例，请注意修改链接后缀
- 2. 查看 CPU 架构可连接 SSH 后执行命令 `uname -ms`，若执行结果是“linux aarch64”，就是搭载的 ARMv8 架构
# 一、 导入 Clash.Meta 内核
Clash.Meta 内核 CPU 架构和链接后缀对应关系如下：
|CPU 架构|AMD64|ARMv5|ARMv6|ARMv7|ARMv8|mips-softfloat|mipsle-hardfloat|mipsle-softfloat|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|链接后缀|`amd64`|`armv5`|`armv6`|`armv7`|`armv8`|`mips-softfloat`|`mipsle-hardfloat`|`mipsle-softfloat`|

如 CPU 架构为 ARMv7，则下载链接须修改为：`https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv7`
和 `https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@release/clash.meta-linux-armv7`  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-arm64 -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8
```
# 二、 导入路由规则文件
连接 SSH 后运行如下命令：
```
curl -o $clashdir/GeoSite.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat
curl -o $clashdir/Country.mmdb -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb
```
# 四、 添加定时任务
连接 SSH 后运行 `crontab -e`，按一下 Ins 键（Insert 键），在最下方粘贴如下内容：
```
30 3 * * * curl -o /data/clash/clash -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8 && chmod +x /data/clash/clash && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 3 点半更新 Clash.Meta 内核
0 4 * * * curl -o /data/clash/GeoSite.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat && curl -o /data/clash/Country.mmdb -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 4 点更新路由规则文件
30 4 * * 1,3,5 /data/clash/start.sh updateyaml && /data/clash/start.sh restart >/dev/null 2>&1 #每周一、三、五早上 4 点半更新订阅并重启 Clash 服务
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车，运行如下命令：
```
/etc/init.d/cron restart
```
# 三、 设置部分
1. 连接 SSH 后运行 `clash` 命令打开 ShellClash 配置脚本  
首次打开会进入新手引导，选择 1 路由设备配置局域网透明代理  
选择 1 在 */data/clash/ui* 目录安装  
根据需要是否选择 1 确认导入配置文件（此处选择 0）  
根据需要是否选择 1 立即启动 clash 服务（此处选择 0）  
输入 0 回车可返回到上级菜单（下同）  
2. 此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核
3. 进入主菜单后选择 9 更新/卸载，进入 7 切换安装源及安装版本，选择 3 公测版 Jsdelivr-CDN 源（推荐）
4. 返回到 9 更新/卸载，进入 4 安装本地 Dashboard 面板，选择 4 安装 Yacd-Meta 魔改面板
5. 返回到主菜单，选择 2 clash功能设置，设置如下：
- 注：有“Tproxy 模式”就选“Tproxy 模式”，否则推荐选“混合模式”
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/717281f6-8cc9-4379-88ef-77a699f58deb" width="60%"/>  

6. 返回到主菜单，进入 4 clash 启动设置，选择 1 允许 clash 开机启动
7. 返回到主菜单，选择 5 设置定时任务，查看定时任务是否添加成功
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/2a2bc646-c5ee-4dd0-841f-bd1383c9eb80" width="60%"/>  

8. 返回到主菜单，选择 7 clash 进阶设置，进入 6 配置内置 DNS 服务，选择 4 一键配置加密（推荐）
9. 返回到主菜单，选择 6 导入配置文件，进入 2 导入 Clash 配置文件链接，粘贴《[生成带有自定义规则和代理组的配置文件 yaml 直链 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geo%20%E6%96%B9%E6%A1%88.md)》中生成的配置文件 .yaml 文件直链，启动 clash 服务即可
10. 访问 Dashboard 面板 [http://192.168.31.1:9999/ui](http://192.168.31.1:9999/ui)，首次打开需要添加“API Base URL”，输入 `http://192.168.31.1:9999` 并点击“Add”，最后点击下方新增的 http://192.168.31.1:9999 即可  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/5c411c5a-8ae4-4e5a-a4f0-364dbf65f57c" width="60%"/>
