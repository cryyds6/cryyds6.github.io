# 使用Docker部署哪吒监控v1 CloudFlare Tunnel版面板端
## 哪吒面板简介
哪吒监控是一款自托管、轻量级的服务器和网站监控运维工具。支持监控系统状态、HTTP（SSL 证书更改、即将过期、过期）、TCP、Ping，并支持推送警报、运行计划任务和 Web 终端。

![哪吒监控官网截图](https://imgbed.20210701.xyz/file/博客/1772523329936_https_nezha.wiki_index.html_哪吒监控_-_服务器监控与运维工具___22495_edit_225923870811210.webp)

**官方网站**：[https://nezha.wiki](https://nezha.wiki)
**官方Github**：[https://github.com/nezhahq/nezha](https://github.com/nezhahq/nezha)
**哪吒v1 Docker CloudFlare Tunnel版Github**：[https://github.com/yumusb/nezha-new](https://github.com/yumusb/nezha-new)

## 哪吒v1 Docker CloudFlare Tunnel版特性
- 不暴露公网IP，防止被攻击
- 单栈转双栈，IPv4/IPv6都能用，纯IPv6服务器也方便挂探针
- 除中国大陆境内网络外，走CF基本都优化
- 开箱即用，迁移与备份方便

## 准备工作
【必需】一台服务器
【cf版必须】域名一枚，并已经接入CloudFlare
**注意**：教程并没有Docker 安装步骤

## 界面演示
![哪吒监控界面演示](https://imgbed.20210701.xyz/file/博客/1772523474329_mark.via_20260303153735.webp)

## 部署哪吒监控
### 1.1 创建Tunnel并保存CloudFlare Tunnel Token
使用CloudFlare账号登入[https://one.dash.cloudflare.com](https://one.dash.cloudflare.com)，然后在左侧边栏找到Tunnel–>网络->连接器

### 1.2 新建CloudFlare Tunnel
点击创建隧道（Create a tunnel）以创建一个新的CloudFlare Tunnel，选择隧道类型为Cloudflared，点击选择Cloudflared进入下一步，为隧道命名后，点击保存隧道（Save tunnel）即可。

### 1.3 提取CloudFlare Tunnel Token
任意环境下复制命令，ey开头的省略字符串就是CloudFlare Tunnel Token，复制命令后自行提取即可。
![提取CloudFlare Tunnel Token](https://imgbed.20210701.xyz/file/博客/1772523610008_Screenshot_20260303_153953_com_hihonor_hnmagicportal_MagicPortalServiceActivity.webp)

### 1.4 开启CloudFlare域名GRPC流量代理
进入Cloudflare，打开已接入的域名页面，点击网络，拉到最下方，将gPRC启用即可。
![开启GRPC流量代理](https://imgbed.20210701.xyz/file/博客/1772523729634_40fbc15e815677d1f36c2ccff4f857a5_edit_226357700589326.webp)

### 2.1 拉取项目并修改配置文件
```bash
git clone https://github.com/yumusb/nezha-new.git
```
进入项目文件夹，编辑 .env 文件，将其中的 TUNNEL_TOKEN 替换为自己申请的CloudFlare Tunnel Token。

### 2.2 启动容器
```bash
docker compose up -d
```

### 2.3 将Dash端映射到CloudFlare
回到[https://one.dash.cloudflare.com](https://one.dash.cloudflare.com)，在左侧边栏找到网络–>Tunnels（Networks > Tunnels），找到之前创建的隧道，点击同一行最右端的三个点，选择配置（Configure）。

### 2.4 添加应用程序路由
点击已发布应用程序路由，点击添加已发布应用程序路由，添加一个自定义域名（主机名称），指向`http://nginx:80`后保存即可。
![添加应用程序路由](https://imgbed.20210701.xyz/file/博客/1772523847783_6711ab7472f30db7085befe7f780bb61_edit_226462923103557.webp)

此时访问在CF穿透的域名，即可正常使用哪吒监控面板！

### 2.5 面板后台基础信息
- 后台地址：/dashboard
- 默认密码：admin/admin

### 3.0 (可选) 探针IP加到CF拦截白名单
由于探针上报日志频繁，且VPS的IP质量参差不齐，可能会被CF误拦截导致无法正常工作，可添加白名单。
操作路径：安全性-WAF-工具
参考文档：[https://developers.cloudflare.com/waf/tools/ip-access-rules/](https://developers.cloudflare.com/waf/tools/ip-access-rules/)

### 4.0 (可选) 配置自动更新
将watchtower相关的注释禁用，修改.env文件中的`TELEGRAM_BOT_TOKEN`、`TELEGRAM_CHAT_ID`，即可实现更新通知。

### 5.0 (可选) 自动备份到Github
将backup相关的注释禁用，修改.env文件中的`GITHUB_USER`、`GITHUB_TOKEN`、`GIT_REMOTE_URL`、`CRON_SCHEDULE`，即可实现自动备份到Github。

## 面板配置
进入后台配置地址`/dashboard/settings`进行如下设置：
1. **Agent对接地址【域名/IP:端口】**：填写上述的 Public hostname:443，并将`Agent 使用 TLS 连接`选项打勾
2. **真实IP请求头**：可填写`nz-realip`或者`CF-Connecting-IP`

### Dashboard更新
进入项目目录下（compose.yml同级），执行以下命令即可更新：
```bash
docker compose pull
docker compose up -d 
```

## AGENT配置
在dashboard右上角复制安装命令，**注意**手动修改参数中的8008端口为443（未修改Agent对接地址时），并将TLS改为True（未修改配置文件中的TLS设置时）。
