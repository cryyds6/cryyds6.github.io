操作步骤
### 1.连接服务器
首先需要判断本机是否有 IPv6 地址，可以通过访问 https://ip.gs 检测是否拥有 IPv6

### 2.添加 IPv4 出口地址
为 VPS 下载或访问一些网络资源，VPS 需要通过网络进行访问，而一些网站如 github 可能只能通过 IPv4 访问，如果 IPv6 的主机没有 连内网 IPv4 都没有，则会访问网络资源失败，影响后续的安装及以后服务的搭建部署。

可以通过如下命令进行尝试，如果 ping 成功，则说明有 IPv4 出口，拥有网络资源的访问能力

```
ping 8.8.8.8 
```
如果无法 ping 通，则需要通过 warp 获取 IPv4 出口地址。

Warp 为 Cloudflare 提供的一种加速网络连接的服务，可以简单理解成，通过 Warp 为服务器套了一个 VPN，来拥有一些新的网络能力。

安装 warp，需要 VPS 开启 tun/tap 功能，执行下面命令，如果返回 tun，则正常，不返回则未开启

```
ls /dev/nat
```
如果没有开启 tun，请自行检索如何开启，开启之后执行下面命令，安装 warp

```
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh
```
命令执行完成会先出现语言选择，按2回车，选择中文，之后会出现一个菜单，菜单项第一项应该如下。

1. 为 IPv6 only 添加 WARP IPv4 网络接口 (bash menu.sh 4) 
作为 IPv6 VPS，输入 1 回车之后，其余选项全都回车默认即可。操作成功会出现大致如下的输出

```
后台获取 WARP IP 中,最大尝试3次……
第1次尝试 
已成功获取 WARP Free 网络, 工作模式: 全局 
==============================================================
IPv4: 104.28.254.16 芬兰  Cloudflare, Inc. 
IPv6: 2a01:4f9:3051:41e7::1b3e:xxxx 芬兰  Hetzner Online GmbH 
恭喜！WARP Free 已开启，总耗时:18秒， 脚本当天运行次数:，累计运行次数: 
IPv6 优先 , 工作模式: 全局 
```
此时除了不能通过 IPv4 访问该机器，则与普通 VPS 无异，拥有 IPv4 出口，及可以通过 IPv6 的全部端口访问该 VPS（前提是访问拥有 IPv6 地址）。

### 3.搭建 1Panel 面板
进入 1Panel 官方安装文档页，根据 VPS 系统，复制指令，笔者的 VPS 系统是 Debian 12，使用如下指令

```
curl -sSL https://resource.fit2cloud.com/1panel/package/quick_start.sh -o quick_start.sh && bash quick_start.sh
```
官网通用一键脚本
```
bash -c "$(curl -sSL https://resource.fit2cloud.com/1panel/package/v2/quick_start.sh)"
```

按照过程中会交互式提示端口、面板入口、用户名及密码，需要注意，请将面板端口设置为 Cloudflare CDN 支持的 http 端口，建议端口如下。

Cloudflare CDN 支持端口
CF CDN 并不是支持所有的端口，笔者选择了端口 8880

HTTP：`80/8080/8880/2052/2082/2086/2095`

HTTPS：`443/2053/2083/2087/2096/8443`

1Panel 搭建成功之后，还需要登入 Cloudflare 管理进行一系列配置操作，才能通过域名访问 1Panel 管理面板。

Cloudflare CDN 配置
首先确认你的域名已经托管在 Cloudflare 上，可以通过 Cloudflare 配置二级域名，基础操作先自行检索

##### 配置 AAAA 类型解析记录
记得打开代理状态，这样通过 Cloudflare 代理 VPS 的 IPv6 地址，同时支持 IPv4 及 IPv6 访问
检查边缘证书的始终使用 HTTPS 选项
因为该选项打开时会导致，从 http 重定向到 https，从而丢失端口，从访问 http://{ipv6domain}.hyx.im:8880/xxx 变成访问 https://{ipv6domain}.hyx.im/1pl。因为此时 1Panel 还未配置域名反向代理，导致无法正常访问面板网站。



此时通过 http://{ipv6domain}.hyx.im:8880/xxx 即可访问 1Panel 登录页。

进阶操作
通过 1Panel 部署网站
1Panel 部署成功之后，即可通过面板快速的创建网站，特别注意需要勾选“监听 IPV6”。



> 本机仅有 IPv4 地址情况下通过 SSH 连接 IPv6 VPS
不建议折腾，操作繁琐，收益较低，在本机无 IPv6 地址情况下，先 SSH 到其他 VPS，再利用其他 VPS 登陆到 IPv6 的 VPS。

`互联网小幻创作，仅供个人使用，如有侵权，请联系删除`