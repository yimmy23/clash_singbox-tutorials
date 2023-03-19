# 前排提示
本教程内容较长，记得点开目录后查看  
<img src="https://user-images.githubusercontent.com/45238096/224132504-d3431fa0-c6db-4e0c-8ab2-ef6f4c99dbec.png" width="60%"/>  

---
# 前言
1. 本教程基于 Redmi AX6000 [官方固件](http://www1.miwifi.com/miwifi_download.html) v1.0.67 版，[ShellClash](https://github.com/juewuy/ShellClash) v1.7.3 版，[AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) v0.108.0 版编写
2. 恢复 SSH，安装 ShellClash 和 AdGuardHome 的方法也适用于其它已解锁 SSH 的路由器
3. 安装 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核和 AdGuardHome 时须注意路由器 CPU 架构，查看 CPU 架构可连接 SSH 后执行如下命令：  
`uname -ms | tr ' ' '_' | tr '[A-Z]' '[a-z]'`  
若执行结果是 linux_aarch64，就下载 armv8 或 arm64 版安装包；若是其它架构请下载相匹配的安装包
4. ShellClash 和 AdGuardHome 中所有没有提到的配置保持默认即可
5. ShellClash 和 AdGuardHome 快速安装方法请看《[ShellClash 和 AdGuardHome 快速安装教程](https://github.com/DustinWin/Router-Plugins/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellClash%20%E5%92%8C%20AdGuardHome%20%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.md)》
---
# 一、 资源下载
打包下载：https://dustinwinvip.lanzoum.com/b01qd6p3a  
密码：zyxz  
注：
- 1. 没有对文件进行任何处理，请自行操作使用
- 2. 不保证实时更新，想用新版请安装后自行升级
- 3. 版本信息请查看打包文件内的 Readme.txt 文本

## 1. ShellClash
官方下载：https://github.com/juewuy/ShellClash/raw/master/bin/ShellClash.tar.gz  
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
nvram set ssh_en=1
nvram set telnet_en=1
nvram set uart_en=1
nvram set boot_wait=on
nvram commit
sed -i 's/channel=.*/channel="debug"/g' /etc/init.d/dropbear
/etc/init.d/dropbear restart
mkdir /data/auto_ssh
cd /data/auto_ssh
curl -O https://fastly.jsdelivr.net/gh/lemoeo/AX6S@main/auto_ssh.sh
chmod +x auto_ssh.sh
uci set firewall.auto_ssh=include
uci set firewall.auto_ssh.type='script'
uci set firewall.auto_ssh.path='/data/auto_ssh/auto_ssh.sh'
uci set firewall.auto_ssh.enabled='1'
uci commit firewall
uci set system.@system[0].timezone='CST-8'
uci set system.@system[0].webtimezone='CST-8'
uci set system.@system[0].timezoneindex='2.84'
uci commit
mtd erase crash
reboot
```
<img src="https://user-images.githubusercontent.com/45238096/226167831-8900620e-7cf9-45aa-a9c9-40d161fb8d65.png" width="60%"/>  

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
sed -i 's/channel=.*/channel="debug"/g' /etc/init.d/dropbear
/etc/init.d/dropbear restart
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
进入控制面板-->程序和功能-->启用或关闭 Windows 功能，勾选“Telnet 客户端”  
<img src="https://user-images.githubusercontent.com/45238096/224110758-b3f85378-39dc-407d-82ba-7b1faaf12753.png" width="60%"/>  

② 启用 OpenSSH 服务器  
进入设置-->应用-->可选功能-->查看功能-->搜索“ssh”，勾选“OpenSSH 服务器”并安装  
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
- 注：若当前电脑登录过 SSH，后路由器经过重新解锁或恢复 SSH，需要进入 *C:\Users\[用户名]\.ssh* 文件夹，删除“known_hosts”文件，否则登录会报错

首次登录需要手动输入“yes”，然后回车  
<img src="https://i.postimg.cc/pVD7KCkq/QQ-20221208153543.png" width="60%"/>  

输入 SSH 登录密码（输入过程中不会显示任何字符），回车  
<img src="https://i.postimg.cc/3RGqSFhr/QQ-20221208153627.png" width="60%"/>  

显示“ARE U OK”表示成功登录 SSH  
<img src="https://i.postimg.cc/9frKTjN7/QQ-20221208153736.png" width="60%"/>  

## 2. 通过 SSH 工具添加 SSH 支持（任选一）
① 打开 Termius  
安装 Termius 并打开（登录后可一直免费试用），现暂时点击“l don't want a free trial”  
<img src="https://i.postimg.cc/wTYzp34b/Snipaste-2022-12-23-13-29-06.png" width="60%"/>  

② 添加 Host  
依次点击 ADD-->New Host  
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
<img src="https://i.postimg.cc/hjYYMZ65/QQ-20230130141719.png" width="60%"/>  

② 连接 SSH，执行如下命令：
```
mkdir -p /tmp/SC_tmp && tar -zxf '/tmp/ShellClash.tar.gz' -C /tmp/SC_tmp/ && source /tmp/SC_tmp/init.sh
```
③ 选择 1 安装到/data 目录（推荐，支持软固化功能）  
④ 将下载的 clash.meta-linux-arm64-xxx.gz 文件解压，得到 clash.meta-linux-arm64 文件  
⑤ 将下载的 upx-xxx-win64.zip 文件解压到桌面，目录结构为 *C:\Users\[用户名]\Desktop\upx*  
⑥ 将 clash.meta-linux-arm64 文件移动到 *C:\Users\[用户名]\Desktop\upx* 文件夹中，以管理员身份运行 PowerShell，依次执行如下命令：
```
cd C:\Users\[用户名]\Desktop\upx
.\upx --best clash.meta-linux-arm64
```
⑦ 将压缩完成的 clash.meta-linux-arm64 重命名为 clash-linux-arm64 并移动到路由器的 */tmp* 目录中  
<img src="https://user-images.githubusercontent.com/45238096/224111734-7454beb0-56e1-4474-b8db-64855582319e.png" width="60%"/>  

⑧ 连接 SSH，进入 ShellClash 配置脚本可执行命令 `clash`，此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核  
<img src="https://i.postimg.cc/YC1w07Zq/QQ-20230204145144.png" width="60%"/>  

**ShellClash 安装成功！**
## 2. ShellClash 配置
① 进入 8 其他工具，选择 2 ShellClash 新手引导  
选择 1 路由设备配置局域网透明代理  
选择 1 在 */data/clash/ui* 目录安装  
选择 1 确认启用软固化 SSH  
根据需要是否选择 1 确认导入配置文件（此处选择 0）  
根据需要是否选择 1 立即启动 clash 服务（此处选择 0）
- 注：强烈建议选择 0，待以下配置完成后，最后一步启动 clash 服务

② 输入 0 回车可返回到上级菜单（下同），返回到主菜单  
进入 2->1 切换 Clash 运行模式，选择 5 Tproxy 模式（部分型号路由器不显示该模式，保持默认即可）  
进入 2 切换 DNS 运行模式，选择 1 fake-ip 模式（经实测，现兼容性已大大增强，如仍有问题请选择 2 redir_host 模式）
- 注：fake-ip 模式不支持 CN_IP 绕过内核

返回到 2 clash 功能设置，进入 6 设置本机代理服务，根据自身需要选择 1 使用 iptables 增强模式配置（支持 docker 推荐！）（不推荐开启）  
注：
- 1. 此功能须重启路由器后生效，作用是使当前路由器的 ShellClash 也走代理，受影响的有生成配置文件（订阅转换）、导入（更新）配置文件、更新脚本和更新 GeoIP/CN-IP 等
- 2. 若生成配置文件（订阅转换）、导入（更新）配置文件、更新脚本和更新 GeoIP/CN-IP 等出现问题，请关闭此功能，并重启路由器后重试
- 3. 若单独使用 ShellClash，推荐设置 DNS 分流，请看《[ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程](https://github.com/DustinWin/Router-Plugins/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B.md)》

<img src="https://user-images.githubusercontent.com/45238096/226165858-fac37651-9f0c-45a0-9a82-9aade5e04e31.png" width="60%"/>  

启用 7 屏蔽 QUIC 流量  
<img src="https://i.postimg.cc/RFCm2YPC/QQ-20230218202031.png" width="60%"/>  

③ 返回到主菜单，进入 4 clash 启动设置  
选择 1 允许 clash 开机启动  
选择 3 设置自启延时，推荐设置成 90 秒（其它型号路由器请自测）  
<img src="https://i.postimg.cc/02h8rcNZ/QQ-20230219114132.png" width="60%"/>  

④ 返回到主菜单，进入 7 clash 进阶设置  
进入 1 ipv6 相关，一般情况下不推荐开启 2 ipv6 透明代理 ，根据自身需要开启 4 CNIP 绕过内核
- 注：fake-ip 模式不支持 CNIP 绕过内核

<img src="https://user-images.githubusercontent.com/45238096/224112024-7b149b2f-9364-4a9e-94a6-0146b5f7445c.png" width="60%"/>  

返回到 7 clash 进阶设置，选择 4 启用域名嗅探  
进入 6 配置内置 DNS 服务  
选择 7 禁用 DNS 劫持  
注：
- 1. 禁用后将无法访问 GitHub 下载 AdGuardHome，请下载完 AdGuardHome 相关工具后再禁用（重要）
- 2. 若单独使用 ShellClash，请不要禁用 DNS 劫持，并强烈建议设置 4 一键配置加密 DNS

<img src="https://i.postimg.cc/8kKkg1Q2/QQ-20230115213642.png" width="60%"/>  

返回到 7 clash 进阶设置，进入 8 手动指定相关端口、秘钥及本机 host  
选择 4 修改 DNS 监听端口，默认即可（此处我修改为：56252）  
选择 5 修改面板访问端口，默认即可（此处我修改为：56253）  
注：
- 1. 修改后访问 ShellClash Dashboard 面板网址变成 http://192.168.31.1:56253/ui
- 2. 首次访问 ShellClash Dashboard 面板需要输入 http://192.168.31.1:56253 ，点击“Add”按钮，添加成功后，点击下方添加成功的网址即可

<img src="https://i.postimg.cc/LXm425sp/QQ-20230102114351.png" width="60%"/>  

<img src="https://i.postimg.cc/0NR2XcRr/QQ-20230115224601.png" width="60%"/>  

⑤ 返回到主菜单，选择 9 更新/卸载，进入 7 切换安装源及安装版本，选择 5 公测版&Jsdelivr-CDN 源（推荐），追求新版可选择 7 内测版（可能不稳定）  
<img src="https://i.postimg.cc/xTZdM96v/QQ-20230202140107.png" width="60%"/>  

⑥ 返回到 9 更新/卸载，进入 3 更新 GeoIP/CN-IP，选择 2 由 Hackl0us 提供的精简版 CN-IP 数据库，等待下载完成  
<img src="https://user-images.githubusercontent.com/45238096/226166467-23c19291-a2bd-4889-a468-ebf8f8da2c9f.png" width="60%"/>  

⑦ 返回到 9 更新/卸载，进入 4 安装本地 Dashboard 面板，选择 4 安装 Yacd-Meta 魔改面板  
<img src="https://i.postimg.cc/DfgJJkV6/QQ-20230306203324.png" width="60%"/>  

⑧ 返回到主菜单，进入 6 导入配置文件  
注：
- 1. 选择 2 导入 Clash 配置文件链接需要一定的 Clash 知识储备，请查看《[生成带有自定义规则和代理组的配置文件.yaml 直链](https://github.com/DustinWin/Router-Plugins/blob/main/README.md)》
- 2. 选择 1 在线生成 Clash 配置文件，可直接转换订阅链接，此处具体方法省略

导入配置文件完成后，根据需要是否选择 1 立即启动 clash 服务，可以选择 1 立即启动，也可以返回到主菜单，输入 0 回车退出脚本并执行如下命令：
```
$clashdir/start.sh start
```
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
$clashdir/start.sh getyaml
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
$clashdir/start.sh stop
clash -u
```
② 通过 ShellClash 配置进行卸载（任选一）  
进入 ShellClash 配置主菜单，进入 9 更新/卸载，选择 9 卸载 ShellClash

# 六 、 AdGuardHome 安装和配置
## 1. AdGuardHome 安装
① 将下载的 upx-xxx-win64.zip 文件解压到桌面，目录结构为 *C:\Users\[用户名]\Desktop\upx*  
② 将下载的 AdGuardHome_linux_arm64.tar.gz 文件复制到桌面，以管理员身份运行 PowerShell，依次执行如下命令：
```
cd C:\Users\[用户名]\Desktop
tar -zxvf AdGuardHome_linux_arm64.tar.gz
```
.tar.gz 压缩文件成功解压到桌面的 *AdGuardHome* 文件夹内，目录结构为 *C:\Users\[用户名]\Desktop\AdGuardHome*  
③ 进入 *AdGuardHome* 文件夹，将里面的“AdGuardHome”文件移动到 *C:\Users\[用户名]\Desktop\upx* 文件夹中  
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
chmod +x /data/auto_ssh
chmod +x /data/auto_ssh/auto_ssh.sh
```
并编辑  
<img src="https://i.postimg.cc/Bvk5zWZH/QQ-20221208162340.png" width="60%"/>  

⑦ 在最下方输入如下内容并保存：
- 注：DNS 服务器监听端口须与命令中的端口保持一致，此处设为 5625（重要）

```
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
```
⑧ 连接 SSH，直接粘贴如下所有命令：
- 注：DNS 服务器监听端口须与命令中的端口保持一致，此处设为 5625（重要）

```
chmod +x /data/AdGuardHome/AdGuardHome
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
```
**AdGuardHome 安装成功！**
## 2. AdGuardHome 配置
① 打开网页 http://192.168.31.1:3000  
点击“开始配置”，“网页管理界面端口”输入“3000”（不建议自定义），“DNS 服务器端口”输入执行安装命令中设置的端口号，如：5625  
“身份认证”设置用户名和密码  
② 点击“打开仪表盘”后输入刚才设置的用户名和密码“登入”，就可以进入 AdGuardHome 管理页面  
③ 进入设置-->常规设置  
取消勾选“启用日志”并点击“保存”（日志非常占用空间）  
④ 进入设置 -->DNS 设置  
“上游 DNS 服务器”设置为：
```
https://dns.alidns.com/dns-query
https://doh.pub/dns-query
tls://dns.google
https://dns.cloudflare.com/dns-query
https://doh.opendns.com/dns-query
```
选中“并行请求”
- 注：此时页面右下角可能会弹出报错信息，但不用理会

<img src="https://i.postimg.cc/FRSb5P1q/QQ-20230228160938.png" width="60%"/>  

“Bootstrap DNS 服务器”设置为：
```
223.5.5.5
119.29.29.29
8.8.8.8
1.1.1.1
```
继续点击“测试上游 DNS”，没有弹出报错信息则证明 AdGuardHome 的上游 DNS 服务器通了，弹出报错信息则修改或删除报错信息中的那个 DNS 地址  
<img src="https://i.postimg.cc/8CjBwkGT/QQ-20230228140830.png" width="60%"/>  

点击“应用”，没有弹出报错信息则证明 AdGuardHome 的上游 DNS 服务器设置没问题，弹出报错信息则修改或删除报错信息中的那个 DNS 地址  
<img src="https://user-images.githubusercontent.com/45238096/224112577-e99ed24d-bd8c-45a9-b05e-1d1c68140813.png" width="60%"/>  

“速度限制”输入“0”，勾选“启用 EDNS 客户端子网”和“启用 DNSSEC”，然后点击下方的“保存”  
<img src="https://user-images.githubusercontent.com/45238096/224112771-5b2d2c4b-dcc3-4520-9c16-5954228bea40.png" width="60%"/>  

勾选“乐观缓存”，并点击“保存”  
<img src="https://i.postimg.cc/CMfrSNWw/QQ-20230115234616.png" width="60%"/>  

⑤ 进入过滤器-->DNS 黑名单，推荐如下列表：
- 注：强烈建议删除自带黑名单并通过“添加黑名单”-->“添加一个自定义列表”进行手动添加，通过“添加黑名单”-->“从列表中选择”进行选择添加容易报错

|名称|URL|
|-----|-----|
|`乘风视频过滤`|`https://cdn.jsdelivr.net/gh/xinggsf/Adblock-Plus-Rule@master/mv.txt`|
|`AdGuard DNS filter`|`https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt`|
|`CHN: AdRules DNS List`|`https://adrules.top/dns.txt`|

- 注：添加后两个（较大）规则时，点击“保存”按钮后需要加载很长时间，如果页面右下角弹出报错信息，直接刷新页面就可以看到该规则已经添加成功

<img src="https://user-images.githubusercontent.com/45238096/224112940-b6c367e0-1ee7-470a-b370-9226c825ffad.png" width="60%"/>  

⑥ 进入过滤器-->DNS 白名单，添加如下列表：
|名称|URL|
|-----|-----|
|`AdRules Allow List`|`https://adrules.top/allow-domains-list.txt`|

<img src="https://i.postimg.cc/YqKn9FHY/Snipaste_2023-03-05_19-22-25.png" width="60%"/>  

⑦ 进入过滤器-->DNS 重写，推荐如下搭配：
|域|IP 地址|
|-----|-----|
|`dns.alidns.com`|`223.5.5.5`|
|`doh.pub`|`119.29.29.29`|
|`dns.google`|`8.8.8.8`|
|`dns.cloudflare.com`|`1.1.1.1`|
|`doh.opendns.com`|`208.67.222.222`|

<img src="https://user-images.githubusercontent.com/45238096/224114271-01bb270f-d753-4e30-af55-61a805387bdc.png" width="60%"/>  

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
chmod +x /data/AdGuardHome/AdGuardHome
reboot
```
## 4. AdGuardHome 卸载
① 删除开机启动项  
执行《六、 1. ⑥》的操作步骤，删除最下方添加的内容：
```
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
```
并保存  
② 卸载 AdGuardHome  
连接 SSH，直接粘贴如下所有命令：
```
/data/AdGuardHome/AdGuardHome -s stop
/data/AdGuardHome/AdGuardHome -s uninstall
rm -rf /data/AdGuardHome
```
③ 重启路由器

# 七、 效果图
## 1. IPv6 效果
![QQ截图20230228143446](https://user-images.githubusercontent.com/45238096/224113189-e20e0b89-6dfc-40c5-b2cf-9abeb8525cdb.png)
## 2. BT 下载效果
UDP 连接正常，使用的是移动 500M 带宽  
![QQ截图20221213012832](https://user-images.githubusercontent.com/45238096/224113233-4d76dec2-495c-4790-a00e-538fc1469639.png)
## 3. ShellClash 效果
使用的是移动 500M 带宽  
[![QQ-20221213022922.png](https://i.postimg.cc/8zG0y6XN/QQ-20221213022922.png)](https://postimg.cc/3dL1NdHc)
## 4. AdGuardHome 效果
![QQ截图20230120190927](https://user-images.githubusercontent.com/45238096/224113288-a84dd015-bb75-402d-82c1-9d71d0c5d7e2.png)
