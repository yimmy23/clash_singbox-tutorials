# 说明：
1. 本教程中 **[AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 安装目录为`/data/AdGuardHome`**
2. 本教程中的下载链接以 CPU 架构 ARMv8 为例，请注意修改链接后缀
3. 查看 CPU 架构可连接 SSH 后执行命令 `uname -ms`，若执行结果是“linux aarch64”，就是搭载的 ARMv8 架构
4. 以下所有命令均可全部复制后直接粘贴执行
---
# 一、 安装 [ShellClash](https://github.com/juewuy/ShellClash)
执行如下命令：
```
curl -o /tmp/ShellClash.tar.gz -L https://cdn.jsdelivr.net/gh/juewuy/ShellClash@master/bin/ShellClash.tar.gz
mkdir -p /tmp/SC_tmp && tar -zxf '/tmp/ShellClash.tar.gz' -C /tmp/SC_tmp/ && source /tmp/SC_tmp/init.sh

```
选择 1 安装到/data 目录（推荐，支持软固化功能）
# 二、 安装 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核
Clash.Meta 内核 CPU 架构和链接后缀对应关系如下：
|CPU 架构|AMD64|ARMv5|ARMv6|ARMv7|ARMv8|mips-softfloat|mipsle-hardfloat|mipsle-softfloat|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|链接后缀|`amd64`|`armv5`|`armv6`|`armv7`|`armv8`|`mips-softfloat`|`mipsle-hardfloat`|`mipsle-softfloat`|

如 CPU 架构为 ARMv7，则下载链接须修改为：  
`https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv7`  
执行如下命令：
```
curl -o /tmp/clash.meta-linux-armv8 -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8 && clash
```
此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核  
或者执行如下命令：
```
curl -o $clashdir/clash -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8
chmod +x $clashdir/clash && $clashdir/start.sh restart

```

# 三、 安装 AdGuardHome
## 1. 安装 AdGuardHome
AdGuardHome CPU 架构和链接后缀对应关系如下：
|CPU 架构|AMD64|ARMv5|ARMv6|ARMv7|ARMv8|mips-softfloat|mipsle-softfloat|
|-----|-----|-----|-----|-----|-----|-----|-----|
|链接后缀|`amd64`|`armv5`|`armv6`|`armv7`|`armv8`|`mips-softfloat`|`mipsle-softfloat`|

如 CPU 架构为 ARMv7，则下载链接须修改为：  
`https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/AdGuardHome/AdGuardHome_linux_armv7`  
执行如下命令：
```
mkdir -p /data/AdGuardHome
curl -o /data/AdGuardHome/AdGuardHome -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/AdGuardHome/AdGuardHome_linux_armv8
chmod +x /data/AdGuardHome/AdGuardHome
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
vi /data/auto_ssh/auto_ssh.sh

```
## 2. 添加开机启动
按一下 Ins 键（Insert 键），在最下方粘贴如下命令：
- 注：保留首尾的空行

```

/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start

```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 3. 升级 AdGuardHome
- 注：留意链接后缀是否与 CPU 架构匹配

执行如下命令：
```
/data/AdGuardHome/AdGuardHome -s stop
curl -o /data/AdGuardHome/AdGuardHome -L https://ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash-tools/main/AdGuardHome/AdGuardHome_linux_armv8
/data/AdGuardHome/AdGuardHome -s start

```
