# 前排提示
本教程内容较长，记得点开目录后查看  
<img src="https://user-images.githubusercontent.com/45238096/224132504-d3431fa0-c6db-4e0c-8ab2-ef6f4c99dbec.png" width="60%"/>

---
# 前言
1. 本教程基于 Redmi AX6000 [官方固件](http://www1.miwifi.com/miwifi_download.html) v1.0.70 版，[ShellClash](https://github.com/juewuy/ShellClash) v1.8.0 版，[AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) v0.108.0 版编写
2. 恢复 SSH，安装 ShellClash 和 AdGuardHome 的方法也适用于其它已解锁 SSH 的路由器
3. 安装 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核和 AdGuardHome 时须注意路由器 CPU 架构，查看 CPU 架构可连接 SSH 后执行如下命令：  
`uname -ms`  
若执行结果是“linux aarch64”，就下载 armv8 或 arm64 版安装包；若是其它架构请下载相匹配的安装包
4. ShellClash 和 AdGuardHome 中所有没有提到的配置保持默认即可
5. ShellClash 和 AdGuardHome 快速安装方法请看《[ShellClash 和 AdGuardHome 快速安装教程](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellClash%20%E5%92%8C%20AdGuardHome%20%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.md)》
6. ShellClash 单独使用时设置 DNS 分流请看《[ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20geo%20%E6%96%B9%E6%A1%88.md)》或《[ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20ruleset%20%E6%96%B9%E6%A1%88.md)》
---
# 一、 资源下载
打包下载：https://dustinwinvip.lanzoum.com/b01qd6p3a  
密码：zyxz  
注：
- 1. 没有对文件进行任何处理，请自行操作使用
- 2. 不保证实时更新，想用新版请安装后自行升级
- 3. 版本信息请查看打包文件内的 Readme.txt 文本

## 1. ShellClash
官方下载：https://raw.githubusercontent.com/juewuy/ShellClash/master/bin/ShellClash.tar.gz
## 2. Clash.Meta 内核
官方下载：https://github.com/MetaCubeX/Clash.Meta/releases  
下载 clash.meta-linux-arm64-xxx.gz 文件  
## 3. Termius
官方下载：https://autoupdate.termius.com/windows/Termius.exe  
## 4. AdGuardHome
官方下载：https://github.com/AdguardTeam/AdGuardHome/releases  
下载 AdGuardHome_linux_arm64.tar.gz 文件  
## 5. UPX
官方下载：https://github.com/upx/upx/releases  
下载 upx-xxx-win64.zip 文件  
## 6. WinSCP
官方下载：https://winscp.net/eng/downloads.php
- 注：中文绿色版请下载打包文件

下载 WinSCP-xxx-Portable.zip 文件
# 二、 解锁 SSH
## 1. 复制 stok 值
进入路由器管理页面 http://192.168.31.1 ，登录后复制地址栏中的 stok 值  
<img src="https://i.postimg.cc/nLjSrGCK/QQ-20230109171055.png" width="60%"/>  

## 2. 开启调试模式
将复制的 stok 值替换如下网址的{stok}并访问：
```
http://192.168.31.1/cgi-bin/luci/;stok={stok}/api/misystem/set_sys_time?timezone=%20%27%20%3B%20zz%3D%24%28dd%20if%3D%2Fdev%2Fzero%20bs%3D1%20count%3D2%202%3E%2Fdev%2Fnull%29%20%3B%20printf%20%27%A5%5A%25c%25c%27%20%24zz%20%24zz%20%7C%20mtd%20write%20-%20crash%20%3B%20
```
网页内容显示{"code":0}表示成功开启调试模式
## 3. 通过浏览器请求重启路由器
继续将复制的 stok 值替换如下网址的{stok}并访问：
```
http://192.168.31.1/cgi-bin/luci/;stok={stok}/api/misystem/set_sys_time?timezone=%20%27%20%3b%20reboot%20%3b%20
```
网页内容显示{"code":0}，此时路由器会重启
## 4. 再次复制 stok 值
重启完成后进入路由器管理页面并登录，再次复制 stok 值
## 5. 设置 Bdata 永久开启 Telnet
将复制的 stok 值替换如下网址的{stok}并访问：
```
http://192.168.31.1/cgi-bin/luci/;stok={stok}/api/misystem/set_sys_time?timezone=%20%27%20%3B%20bdata%20set%20telnet_en%3D1%20%3B%20bdata%20set%20ssh_en%3D1%20%3B%20bdata%20set%20uart_en%3D1%20%3B%20bdata%20commit%20%3B%20
```
网页内容显示{"code":0}表示成功设置 Bdata 永久开启 Telnet
## 6. 再次通过浏览器请求重启路由器
继续将复制的 stok 值替换如下网址的{stok}并访问：
```
http://192.168.31.1/cgi-bin/luci/;stok={stok}/api/misystem/set_sys_time?timezone=%20%27%20%3b%20reboot%20%3b%20
```
网页内容显示{"code":0}，此时路由器会再次重启
## 7. 连接 Telnet
显示“ARE U OK”表示成功解锁 SSH  
<img src="https://i.postimg.cc/DZ1CdfQ8/QQ-20230102101630.png" width="60%"/>  

## 8. 永久开启并固化 SSH
直接粘贴如下所有命令：
- 注：第一行命令是将 Telnet 或 SSH 登录密码设置为“12345678”，可自定义

```
echo -e '12345678\n12345678' | passwd root
nvram set telnet_en=1
nvram set ssh_en=1
nvram set uart_en=1
nvram set boot_wait=on
nvram commit
/etc/init.d/dropbear enable & /etc/init.d/dropbear start
mkdir -p /data/auto_ssh
curl -o /data/auto_ssh/auto_ssh.sh -L https://fastly.jsdelivr.net/gh/lemoeo/AX6S@main/auto_ssh.sh
chmod +x /data/auto_ssh/auto_ssh.sh
/data/auto_ssh/auto_ssh.sh install
uci set system.@system[0].timezone='CST-8'
uci set system.@system[0].webtimezone='CST-8'
uci set system.@system[0].timezoneindex='2.84'
uci commit
mtd erase crash
reboot
```
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/6b3af81b-94ac-4905-b05b-b6fd98a513a2" width="60%"/>  

最后一行 reboot 命令需要手动回车（下同），回车后路由器会重启  
**SSH 解锁成功！**
# 三、 恢复 SSH
若已解锁并固化过 SSH 的路由器在升级固件或恢复出厂设置后 SSH 丢失，可快速再次解锁 SSH
## 1. 计算 Telnet 登录密码
打开网站 https://miwifi.dev/ssh ，在 SN 处输入路由器背面的 SN 号，点击“Calc”按钮  
<img src="https://i.postimg.cc/X74PL5z3/QQ-20221208192756.png" width="60%"/>  

## 2. 连接 Telnet
用户名为：root，密码为第 1 步中计算出的 Telnet 登录密码  
<img src="https://user-images.githubusercontent.com/45238096/224110394-e61c7373-f944-49b7-95d2-af18e31809ce.png" width="60%"/>  

直接粘贴如下所有命令：
- 注：最后一行命令是将 Telnet 或 SSH 登录密码设置为“12345678”，可自定义

```
mkdir -p /data/auto_ssh
curl -o /data/auto_ssh/auto_ssh.sh -L https://fastly.jsdelivr.net/gh/lemoeo/AX6S@main/auto_ssh.sh
chmod +x /data/auto_ssh/auto_ssh.sh
/data/auto_ssh/auto_ssh.sh install
echo -e '12345678\n12345678' | passwd root
```
## 3. 更改 Telnet 和 SSH 登录密码（可选）
执行如下命令：  
```
passwd root
```
输入密码如：12341234，回车后输入同样的密码，再次回车即可  
**SSH 恢复成功！**
# 四、 连接 SSH
## 1. 给 Windows 操作系统添加 SSH 支持（任选一）
① 启用 Telnet 客户端  
进入控制面板->程序和功能->启用或关闭 Windows 功能，勾选“Telnet 客户端”  
<img src="https://user-images.githubusercontent.com/45238096/224110758-b3f85378-39dc-407d-82ba-7b1faaf12753.png" width="60%"/>  

② 启用 OpenSSH 服务器  
进入设置->应用->可选功能->查看功能->搜索“ssh”，勾选“OpenSSH 服务器”并安装  
<img src="https://user-images.githubusercontent.com/45238096/224110859-c869fed4-05bb-495b-a13c-aa3f78bb0ef7.png" width="60%"/>  

重启系统  
③ 连接 Telnet  
成功解锁或恢复 SSH 后，以管理员身份运行 PowerShell 或 CMD，执行如下命令：
```
telnet 192.168.31.1
```
- 注：首次登录不需要用户名和密码，解锁或恢复 SSH 后用户名为 root，密码为 SSH 登录密码

④ 连接 SSH  
成功解锁或恢复 SSH 后，以管理员身份运行 PowerShell 或 CMD，执行如下命令：
```
ssh root@192.168.31.1
```
- 注：若当前电脑登录过 SSH，后路由器经过重新解锁或恢复 SSH，需要进入 *C:\Users\\[用户名]\\.ssh* 文件夹，删除“known_hosts”文件，否则登录会报错

首次登录需要手动输入“yes”，然后回车  
<img src="https://i.postimg.cc/pVD7KCkq/QQ-20221208153543.png" width="60%"/>  

输入 SSH 登录密码（输入过程中不会显示任何字符），回车  
<img src="https://i.postimg.cc/3RGqSFhr/QQ-20221208153627.png" width="60%"/>  

显示“ARE U OK”表示成功登录 SSH  
<img src="https://i.postimg.cc/9frKTjN7/QQ-20221208153736.png" width="60%"/>  

## 2. 通过 SSH 工具添加 SSH 支持（任选一）
① 打开 Termius  
安装 Termius 并打开（登录后可一直免费试用），现暂时点击“l don't want a free trial”  
<img src="https://user-images.githubusercontent.com/45238096/231961082-1fb3b895-e42d-46d5-98a6-e7a15cdaa2cc.png" width="60%"/>  

② 添加 Host  
依次点击 ADD->New Host  
<img src="https://user-images.githubusercontent.com/45238096/224111075-edf1f8a5-d30a-4c95-823f-0a756d2a9565.png" width="60%"/>  

③ 添加 Telnet  
按图输入，选中“Telnet”，点击右上角的“→|”图标
- 注：首次登录不需要用户名和密码，解锁或恢复 SSH 后用户名为 root，密码为 SSH 登录密码

<img src="https://user-images.githubusercontent.com/45238096/224111315-e5d6caa3-962d-4b1e-9b2d-2a917bb527c2.png" width="60%"/>  

④ 添加 SSH  
同样先按照第②步操作，然后按图输入，选中“SSH”，“Password”为解锁或恢复 SSH 时设置的密码，最后一步点击右上角的“→|”图标  
<img src="https://user-images.githubusercontent.com/45238096/224111529-4eab34c8-4c28-41d3-8a20-bb62ad61ab84.png" width="60%"/>  

⑤ 连接 Telnet 或 SSH  
按需双击添加的项即可  
<img src="https://i.postimg.cc/pXK2hKnH/Snipaste_2022-12-23_13-32-27.png" width="60%"/>  

- 注：首次连接 SSH 需要点击“Add and continue”

<img src="https://i.postimg.cc/Jny81XWS/Snipaste_2022-12-23_13-32-54.png" width="60%"/>  

显示“ARE U OK”表示成功登录 SSH  
<img src="https://i.postimg.cc/Bv7s9gRx/QQ-20230102102435.png" width="60%"/>  

## 3. 通过 WinSCP 连接路由器文件管理
将下载的 WinSCP-xxx-Portable.zip 文件解压，路径随意，打开 WinSCP，“文件协议”选择“SCP”，其它按图输入，“密码”为 SSH 登录密码，完成后点击登录  
<img src="https://i.postimg.cc/vT1Jsw5y/QQ-20221208160309.png" width="60%"/>  

左侧为电脑本地文件，右侧为路由器文件  
<img src="https://i.postimg.cc/YCkt1j1m/QQ-20230130134908.png" width="60%"/>  

# 五、 ShellClash 安装和配置
## 1. ShellClash 安装
① 打开 WinSCP，将下载的 ShellClash.tar.gz 文件移动到路由器的 */tmp* 目录中  
<img src="https://user-images.githubusercontent.com/45238096/227115154-1aa926a7-ba65-4d5a-ae5c-5af37e0ed85e.png" width="60%"/>  

② 连接 SSH，执行如下命令：
```
mkdir -p /tmp/SC_tmp && tar -zxf '/tmp/ShellClash.tar.gz' -C /tmp/SC_tmp/ && source /tmp/SC_tmp/init.sh
```
③ 选择 1 安装到/data 目录（推荐，支持软固化功能）  
④ 将下载的 clash.meta-linux-arm64-xxx.gz 文件解压，得到 clash.meta-linux-arm64 文件  
⑤ 将下载的 upx-xxx-win64.zip 文件解压到桌面，目录结构为 *C:\Users\\[用户名]\Desktop\upx*  
⑥ 将 clash.meta-linux-arm64 文件移动到 *C:\Users\\[用户名]\Desktop\upx* 文件夹中，以管理员身份运行 PowerShell，依次执行如下命令：
```
cd C:\Users\[用户名]\Desktop\upx
.\upx --best clash.meta-linux-arm64
```
⑦ 将压缩完成的 clash.meta-linux-arm64 移动到路由器的 */tmp* 目录中  
<img src="https://user-images.githubusercontent.com/45238096/230613949-da4d28e2-0654-4dd4-b781-f7e5b8910b98.png" width="60%"/>  

**ShellClash 安装成功！**
## 2. ShellClash 配置
① 连接 SSH 后运行 `clash` 命令打开 ShellClash 配置脚本  
首次打开会进入新手引导，选择 1 路由设备配置局域网透明代理  
根据需要是否启用软固化（此处选择 0）
- 注：解锁 SSH 时已成功启用软固化

根据需要是否选择 1 确认导入配置文件（此处选择 0）  
根据需要是否选择 1 立即启动 clash 服务（此处选择 0）
- 注：强烈建议选择 0，待以下配置完成后，最后一步启动 clash 服务

② 此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/e88587df-785c-459f-88ac-940e3d858a7c" width="60%"/>  

③ 进入主菜单->2 clash 功能设置->1 切换 Clash 运行模式，选择 5 Tproxy 模式
- 注：有“Tproxy 模式”就选“Tproxy 模式”，否则推荐选“混合模式”，宽带在 300M 内推荐 Tun 模式

进入 2 切换 DNS 运行模式，选择 1 fake-ip 模式（经实测，现兼容性已大大增强，如仍有问题请选择 2 redir_host 模式）
- 注：fake-ip 模式不支持 CN_IP 绕过内核

进入 6 设置本机代理服务，选择 1 使用 iptables 增强模式配置
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/4e7874ca-af54-4dd4-b6cc-a97b3a34ed8d" width="60%"/>  

启用 7 屏蔽 QUIC 流量  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/a6416c9e-7ba9-44c0-8f95-adc1982e23d3" width="60%"/>  

④ 返回到主菜单，进入 4 clash 启动设置，选择 1 允许 clash 开机启动  
⑤ 返回到主菜单，进入 7 clash 进阶设置  
进入 1 ipv6 相关，一般情况下不推荐开启 2 ipv6 透明代理 ，根据自身需要开启 4 CNIP 绕过内核
- 注：fake-ip 模式不支持 CNIP 绕过内核

<img src="https://user-images.githubusercontent.com/45238096/224112024-7b149b2f-9364-4a9e-94a6-0146b5f7445c.png" width="60%"/>  

返回到 7 clash 进阶设置，根据自身需要选择 4 启用域名嗅探（若全配置加密 DNS 则不用开启）  
根据自身需要选择 5 启用节点绕过（设备较多可开启）  
进入 6 配置内置 DNS 服务，选择 7 禁用 DNS 劫持
注：
- 1. 若单独使用 ShellClash，请不要禁用 DNS 劫持
- 2. 推荐设置 DNS 分流（单独使用 ShellClash 以及 ShellClash 搭配 AdGuardHome 都适用），请看《[ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20geo%20%E6%96%B9%E6%A1%88.md)》或《[ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20ruleset%20%E6%96%B9%E6%A1%88.md)》

<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/eb80990b-cb92-4038-a766-d6c691ec266d" width="60%"/>  

⑥ 返回到主菜单，选择 9 更新/卸载，进入 7 切换安装源及安装版本，选择 3 公测版 Jsdelivr-CDN 源（推荐），追求新版可选择 7 内测版（可能不稳定）  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/28dd2543-97f8-4bb4-bc77-0f024afa5a88" width="60%"/>  

⑦ 返回到 9 更新/卸载，进入 4 安装本地 Dashboard 面板，选择 4 安装 Yacd-Meta 魔改面板  
- 注：
- 1. 启动 Clash 服务后，面板 Dashboard 访问链接为：http://192.168.31.1:9999/ui
- 2. 初次打开需要添加链接：http://192.168.31.1:9999

<img src="https://i.postimg.cc/DfgJJkV6/QQ-20230306203324.png" width="60%"/>  

⑧ 返回到主菜单，再次进入 6 导入配置文件  
注：
- 1. 选择 1 在线生成 Clash 配置文件，粘贴你的订阅链接并回车，输入“1”并再次回车即可
- 2. 选择 2 导入 Clash 配置文件链接，需要一定的 Clash 知识储备，请查看《[生成带有自定义规则和代理组的配置文件 yaml 直链 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geo%20%E6%96%B9%E6%A1%88.md)》或《[生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20ruleset%20%E6%96%B9%E6%A1%88.md)》

导入配置文件完成后，根据需要是否选择 1 立即启动 clash 服务，此处选择 0 **不启动 clash 服务**  
**ShellClash 配置成功！**  
**拓展：**  
ShellClash 常用命令
- 1. 打开配置：
```
clash
```
- 2. 启动服务：
```
$clashdir/start.sh start
```
- 3. 停止服务：
```
$clashdir/start.sh stop
```
- 4. 重启服务：
```
$clashdir/start.sh restart
```
- 5. 更新订阅：
```
$clashdir/start.sh updateyaml
```
- 6. 查看帮助和说明：
```
clash -h
```
## 3. ShellClash 升级
进入 ShellClash 配置主菜单，选择 9 更新/卸载，进入后查看“管理脚本”、“clash 核心”和“GeoIP/CN-IP”有无新版本，有则选择对应的数字进行升级即可  
<img src="https://i.postimg.cc/R0HJNW9J/QQ-20221223141923.png" width="60%"/>  

## 4. ShellClash 卸载
① 通过脚本命令进行卸载（任选一）  
连接 SSH 后，直接粘贴如下所有命令：
```
$clashdir/start.sh stop && clash -u
```
② 通过 ShellClash 配置进行卸载（任选一）  
进入 ShellClash 配置主菜单，进入 9 更新/卸载，选择 9 卸载 ShellClash
# 六 、 AdGuardHome 安装和配置
## 1. AdGuardHome 安装
① 将下载的 upx-xxx-win64.zip 文件解压到桌面，目录结构为 *C:\Users\\[用户名]\Desktop\upx*  
② 将下载的 AdGuardHome_linux_arm64.tar.gz 文件复制到桌面，以管理员身份运行 PowerShell，依次执行如下命令：
```
cd C:\Users\[用户名]\Desktop
tar -zxvf AdGuardHome_linux_arm64.tar.gz
```
.tar.gz 压缩文件成功解压到桌面的 *AdGuardHome* 文件夹内，目录结构为 *C:\Users\\[用户名]\Desktop\AdGuardHome*  
③ 进入 *AdGuardHome* 文件夹，将里面的“AdGuardHome”文件移动到 *C:\Users\\[用户名]\Desktop\upx* 文件夹中  
此时“AdGuardHome”文件大小：  
<img src="https://i.postimg.cc/8cBhsjRm/QQ-20221216151642.png" width="60%"/>  

④ 依次执行如下命令：
```
cd C:\Users\[用户名]\Desktop\upx
.\upx --best AdGuardHome
```
等待压缩完成，完成后“AdGuardHome”文件大小：  
<img src="https://user-images.githubusercontent.com/45238096/224112398-45ce73d6-1c9c-4707-9d01-0ac57eb26c43.png" width="60%"/>  

⑤ 将压缩后的“AdGuardHome”文件移动到路由器的 */data/AdGuardHome* 目录（没有此目录就新建）中  
<img src="https://i.postimg.cc/qvZ2tf3S/QQ-20230130144006.png" width="60%"/>  

⑥ 进入路由器文件管理的 */data/auto_ssh* 目录，右击“auto_ssh.sh”文件  
注：
- 1. 若没有此目录和文件，可新建
- 2. 新建后连接 SSH，直接粘贴如下所有命令：

```
chmod +x /data/auto_ssh && chmod +x /data/auto_ssh/auto_ssh.sh
```
并编辑  
<img src="https://i.postimg.cc/Bvk5zWZH/QQ-20221208162340.png" width="60%"/>  

⑦ 在 `unlock()` 上方输入如下内容并保存：
- 注：AdGuardHome 的 “DNS 服务器端口”须设置为“5353”
```
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5353
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5353
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5353
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5353
```
⑧ 连接 SSH，直接粘贴如下所有命令：
- 注：AdGuardHome 的 “DNS 服务器端口”须设置为“5353”
```
chmod +x /data/AdGuardHome/AdGuardHome
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5353
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5353
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5353
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5353
```
**AdGuardHome 安装成功！**
## 2. AdGuardHome 配置
① 打开网页 http://192.168.31.1:3000  
点击“开始配置”，**“网页管理界面端口”输入“3000”，“DNS 服务器端口”输入“5353”**  
“身份认证”设置用户名和密码  
② 点击“打开仪表盘”后输入刚才设置的用户名和密码“登入”，就可以进入 AdGuardHome 管理页面  
③ 进入设置->常规设置  
取消勾选“启用日志”并点击“保存”（日志非常占用空间）  
④ 进入设置 ->DNS 设置  
“上游 DNS 服务器”设置为 `127.0.0.1:1053`，选中“并行请求”
- 注：此时页面右下角可能会弹出报错信息，但不用理会

<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/1a3065d0-3d4d-4fff-91e1-bdf21344fefc" width="60%"/>  

“Bootstrap DNS 服务器”设置为：
```
1.12.12.12
223.5.5.5
```
直接点击“应用”即可  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/d32e40c3-d436-4076-a2bf-c622707827ba" width="60%"/>  

“速度限制”输入“0”，勾选“启用 EDNS 客户端子网”和“启用 DNSSEC”，然后点击下方的“保存”  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/8b43ed45-afec-4444-b8fc-77d784477649" width="60%"/>  

勾选“乐观缓存”，并点击“保存”  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/f857800a-ebf4-4031-994f-27cd6b3e7b53" width="60%"/>  

⑤ 进入过滤器->DNS 黑名单，推荐如下列表：
- 注：强烈建议删除自带黑名单并通过“添加黑名单”->“添加一个自定义列表”进行手动添加，通过“添加黑名单”->“从列表中选择”进行选择添加容易报错

|名称|URL|
|-----|-----|
|`CHN: anti-AD`|`https://anti-ad.net/easylist.txt`|

- 注：添加较大规则时，点击“保存”按钮后需要加载很长时间，如果页面右下角弹出报错信息，直接刷新页面就可以看到该规则已经添加成功

<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/5dd46dc8-3fd8-4eb8-8d7b-377fa138c777" width="60%"/>  

**AdGuardHome 配置成功！**  
**拓展：**  
AdGuardHome 常用命令
- 1. 启动服务：
```
/data/AdGuardHome/AdGuardHome -s start
```
- 2. 停止服务：
```
/data/AdGuardHome/AdGuardHome -s stop
```
- 3. 重启服务：
```
/data/AdGuardHome/AdGuardHome -s restart
```
- 4. 显示当前服务状态：
```
/data/AdGuardHome/AdGuardHome -s status
```
## 3. AdGuardHome 升级
为了节约路由器内存，请按照如下步骤进行操作：  
① 执行《六、 1. ① ② ③ ④ ⑤（替换）》的操作步骤  
② 连接 SSH，直接粘贴如下所有命令：
```
chmod +x /data/AdGuardHome/AdGuardHome && reboot
```
## 4. AdGuardHome 卸载
① 删除开机启动项  
执行《六、 1. ⑥》的操作步骤，删除添加的内容：
```
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5353
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5353
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5353
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5353
```
并保存  
② 卸载 AdGuardHome  
连接 SSH，直接粘贴如下所有命令：
```
/data/AdGuardHome/AdGuardHome -s stop && /data/AdGuardHome/AdGuardHome -s uninstall && rm -rf /data/AdGuardHome
```
③ 重启路由器
# 七、 效果图
## 1. IPv6 效果
<img src="https://user-images.githubusercontent.com/45238096/224113189-e20e0b89-6dfc-40c5-b2cf-9abeb8525cdb.png" width="100%"/>  

## 2. BT 下载效果
UDP 连接正常，使用的是移动 500M 带宽  
<img src="https://user-images.githubusercontent.com/45238096/224113233-4d76dec2-495c-4790-a00e-538fc1469639.png" width="100%"/>  

## 3. ShellClash 效果
使用的是移动 500M 带宽  
<img src="https://i.postimg.cc/8zG0y6XN/QQ-20221213022922.png)](https://postimg.cc/3dL1NdHc" width="100%"/>  

## 4. AdGuardHome 效果
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/d37f6157-0684-44af-99b7-871280343be9" width="100%"/>
