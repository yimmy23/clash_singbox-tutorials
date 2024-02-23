# [ShellCrash](https://github.com/juewuy/ShellCrash) 配置-ruleset  方案
注：
- 1. 此方案此方案适用于 [sing-box](https://github.com/PuerNya/sing-box)，采用 `rule_set` 规则
- 2. 本教程中的下载链接以 CPU 架构 ARMv8 为例，请注意修改链接后缀
- 3. 查看 CPU 架构可连接 SSH 后执行命令 `uname -ms`，若执行结果是“linux aarch64”，就是搭载的 ARMv8 架构
---
# 一、 导入 [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)
**sing-box 内核下载链接后缀和 CPU 架构对应关系如下：**
|CPU 架构|AMD64|AMD64v3|ARMv5|ARMv6|ARMv7|ARMv8&ARM64&AArch64|mips-softfloat|mipsle-hardfloat|mipsle-softfloat|
|-----|-----|-----|-----|-----|-----|:---:|-----|-----|-----|
|**链接后缀**|`amd64`|`amd64v3`|`armv5`|`armv6`|`armv7`|`armv8`|`mips-softfloat`|`mipsle-hardfloat`|`mipsle-softfloat`|

连接 SSH 后执行如下命令：
```
curl -o /tmp/sing-box-linux-armv8 -L https://fastly.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-puernya/sing-box-linux-armv8 && crash
```
此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 5 Sing-Box-Puer 内核
# 二、 添加定时任务
连接 SSH 后执行 `vi $CRASHDIR/task/task.user`，按一下 Ins 键（Insert 键），粘贴（快捷键 Ctrl+Shift+V）如下内容：
- 注：ShellCrash 安装路径为 */data/ShellCrash*

```
201#curl -o /data/ShellCrash/CrashCore.tar.gz -L https://fastly.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-puernya/sing-box-linux-armv8.tar.gz && /data/ShellCrash/start.sh restart >/dev/null 2>&1#更新sing-box_PuerNya版内核
202#curl -o /tmp/metacubexd.tar.gz -L https://fastly.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/Clash-dashboard/metacubexd.tar.gz && rm -rf /data/ShellCrash/ui/* && tar -zxf /tmp/metacubexd.tar.gz -C /data/ShellCrash/ui && rm -f /tmp/metacubexd.tar.gz && /data/ShellCrash/start.sh restart >/dev/null 2>&1#更新metacubexd面板
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
# 三、 设置部分
1. 连接 SSH 后执行 `crash` 命令打开 ShellCrash 配置脚本  
首次打开会进入新手引导，选择 1 路由设备配置局域网透明代理  
启用推荐的自动任务配置  
根据需要是否启用软固化（此处选择 1）  
根据需要是否选择 1 确认导入配置文件（此处选择 0）  
根据需要是否选择 1 立即启动 clash 服务（此处选择 0）  
输入 0 回车可返回到上级菜单（下同）  
2. 此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 5 Sing-Box-Puer 内核
3. 进入主菜单-> 9 更新/卸载-> 7 切换安装源及安装版本，选择 b 切换至公测版->1 Jsdelivr_CDN源（推荐）
4. 进入主菜单->9 更新/卸载->4 安装本地 Dashboard 面板，选择 3 安装 MetaXD 面板
5. 进入主菜单->2 内核功能设置，设置如下：
- 注：有“Tproxy 模式”就选“Tproxy 模式”，否则推荐选“混合模式”，宽带在 300M 内推荐 Tun 模式
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/31595cf6-d1d8-4def-bc96-c1da521a4d0d" width="60%"/>

6. 进入主菜单->4 内核启动设置，选择 1 允许 ShellCrash 开机启动（若重启路由器后服务没有自动运行，可“设置自启延时”为 30 秒）
7. 进入主菜单->7 内核进阶设置，启用 4 启用域名嗅探
8. 进入主菜单->7 内核进阶设置->6 配置内置 DNS 服务，选择 4 一键配置加密（推荐）
9. 进入主菜单->6 导入配置文件->2 在线获取完整配置文件，粘贴《[生成带有自定义策略组和规则的 sing-box 配置文件直链-ruleset 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md)》中生成的配置文件 .json 文件直链，启动服务即可
10. 访问 Dashboard 面板 [http://192.168.31.1:9999/ui](http://192.168.31.1:9999/ui)，首次打开需要添加“后端地址”，输入 `http://192.168.31.1:9999` 并点击“添加”即可  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/2785f9a1-4d64-435c-864c-c0dcd4eb69fc" width="60%"/>

11. 进入 Dashboard 面板->代理->代理提供者，点击“转圈图标”（Update），也可手动更新节点
