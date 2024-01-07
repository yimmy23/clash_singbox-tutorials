# [ShellClash](https://github.com/juewuy/ShellCrash) 配置-geox 方案
注：
- 1. 此方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） [路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
- 2. 本教程中的下载链接以 CPU 架构 ARMv8 为例，请注意修改链接后缀
- 3. 查看 CPU 架构可连接 SSH 后执行命令 `uname -ms`，若执行结果是“linux aarch64”，就是搭载的 ARMv8 架构
---
# 一、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/mihomo)
Clash.Meta 内核 CPU 架构和链接后缀对应关系如下：
|CPU 架构|AMD64|ARMv5|ARMv6|ARMv7|ARMv8|mips-softfloat|mipsle-hardfloat|mipsle-softfloat|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|**链接后缀**|`amd64`|`armv5`|`armv6`|`armv7`|`armv8`|`mips-softfloat`|`mipsle-hardfloat`|`mipsle-softfloat`|

如 CPU 架构为 ARMv7，则下载链接须修改为：`https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv7`  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-arm64 -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8 && crash
```
此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核
# 二、 导入路由规则文件
连接 SSH 后运行如下命令：
```
curl -o $CRASHDIR/GeoSite.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat
curl -o $CRASHDIR/Country.mmdb -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb
```
# 三、 添加定时任务
连接 SSH 后运行 `vi $CRASHDIR/task/task.user`，按一下 Ins 键（Insert 键），粘贴（快捷键 Ctrl+Shift+V）如下内容：
```
201#30 3 * * 1,3,5 curl -o /data/clash/clash -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8 && chmod +x /data/clash/clash && /data/clash/start.sh restart >/dev/null 2>&1#每周一、三、五早上 3 点半更新 Clash.Meta 内核
202#0 4 * * * curl -o /data/clash/GeoSite.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat && curl -o /data/clash/Country.mmdb -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb && /data/clash/start.sh restart >/dev/null 2>&1#每天早上 4 点更新路由规则文件
203#30 4 * * 1,3,5 curl -o /tmp/metacubexd.tar.gz -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash-dashboard/metacubexd.tar.gz && rm -rf /data/clash/ui/* && tar -zxf /tmp/metacubexd.tar.gz -C /data/clash/ui && rm -f /tmp/metacubexd.tar.gz && /data/clash/start.sh restart >/dev/null 2>&1#每周一、三、五早上 4 点半更新 metacubexd 面板
```
按一下 Esc 键（退出键），输入英文冒号`:`，继续输入 `wq` 并回车
# 四、 设置部分
1. 连接 SSH 后运行 `crash` 命令打开 ShellClash 配置脚本  
首次打开会进入新手引导，选择 1 路由设备配置局域网透明代理  
启用推荐的自动任务配置  
根据需要是否启用软固化（此处选择 0）
- 注：解锁 SSH 时已成功启用软固化

根据需要是否选择 1 确认导入配置文件（此处选择 0）  
根据需要是否选择 1 立即启动 clash 服务（此处选择 0）  
输入 0 回车可返回到上级菜单（下同）  
3. 此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核
4. 进入主菜单->9 更新/卸载->7 切换安装源及安装版本，选择 3 公测版 Jsdelivr-CDN 源（推荐）
5. 进入主菜单->9 更新/卸载->4 安装本地 Dashboard 面板，选择 5 安装 MetaXD 面板
6. 进入主菜单->2 内核功能设置，设置如下：
- 注：有“Tproxy 模式”就选“Tproxy 模式”，否则推荐选“混合模式”，宽带在 300M 内推荐 Tun 模式  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/717281f6-8cc9-4379-88ef-77a699f58deb" width="60%"/>

6. 进入主菜单->4 内核启动设置，选择 1 允许 clash 开机启动
7. 进入主菜单->7 内核进阶设置->6 配置内置 DNS 服务，选择 4 一键配置加密（推荐）
8. 进入主菜单->6 导入配置文件->2 导入 Clash 配置文件链接，粘贴《[生成带有自定义策略组和规则的 yaml 配置文件直链-geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20yaml%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geox%20%E6%96%B9%E6%A1%88.md)》中生成的配置文件 .yaml 文件直链，启动 clash 服务即可
9. 访问 Dashboard 面板 [http://192.168.31.1:9999/ui](http://192.168.31.1:9999/ui)，首次打开需要添加“后端地址”，输入 `http://192.168.31.1:9999` 并点击“添加”即可  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/91ebae62-df79-4d1d-9998-d47adb69cf43" width="60%"/>

10. 进入 Dashboard 面板->代理->代理提供者，点击“转圈图标”（Update），也可手动更新节点
