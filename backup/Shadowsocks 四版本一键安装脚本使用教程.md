# Shadowsocks 四版本一键安装脚本使用教程

## 本脚本适用环境
系统支持：CentOS 6+，Debian 7+，Ubuntu 12+
内存要求：≥128M
日期：2020 年 06 月 01 日

## 默认配置
服务器端口：自己设定（如不设定，默认从 9000-19999 之间随机生成）
密码：自己设定（如不设定，默认为 teddysun.com）
加密方式：自己设定（如不设定，Python 和 libev 版默认为 aes-256-gcm，R 和 Go 版默认为 aes-256-cfb）
协议（protocol）：自己设定（如不设定，默认为 origin）（仅限 ShadowsocksR 版）
混淆（obfs）：自己设定（如不设定，默认为 plain）（仅限 ShadowsocksR 版）

备注：脚本默认创建单用户配置文件，如需配置多用户，请手动修改相应的配置文件后重启即可。

## 使用方法
使用 root 用户登录，运行以下命令：
```bash
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

## 安装完成后脚本提示示例
```
Congratulations, your_shadowsocks_version install completed!
Your Server IP        :your_server_ip
Your Server Port      :your_server_port
Your Password         :your_password
Your Encryption Method:your_encryption_method

Your QR Code: (For Shadowsocks Windows, OSX, Android and iOS clients)
 ss://your_encryption_method:your_password@your_server_ip:your_server_port
Your QR Code has been saved as a PNG file path:
 your_path.png

Welcome to visit:https://teddysun.com/486.html
Enjoy it!
```

## 客户端下载
Shadowsocks 客户端下载：
- 安卓：[https://github.com/shadowsocks/shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android)
- Windows：[https://github.com/shadowsocks/shadowsocks-windows](https://github.com/shadowsocks/shadowsocks-windows)

v2rayNG 客户端下载：
- Windows 客户端：[https://github.com/2dust/v2rayN/releases](https://github.com/2dust/v2rayN/releases)
- Android 客户端：[https://github.com/2dust/v2rayNG/releases](https://github.com/2dust/v2rayNG/releases)

## 卸载方法
若已安装多个版本，则卸载时也需多次运行（每次卸载一种）。

使用 root 用户登录，运行：
```bash
./shadowsocks-all.sh uninstall
```

## 启动脚本
启动脚本后面的参数含义，从左至右依次为：启动，停止，重启，查看状态。

**Shadowsocks-Python 版R 版Go 版ibev 版：**
```bash
/etc/init.d/shadowsocks-python start | stop | restart | status
```
```bash
/etc/init.d/shadowsocks-r start | stop | restart | status
```
```bash
/etc/init.d/shadowsocks-go start | stop | restart | status
```
```bash
/etc/init.d/shadowsocks-libev start | stop | restart | status
```

## 默认配置文件
```text
/etc/shadowsocks/config.json
```

# Alpine Linux v3.20 系统
### 解决方案

#### 方案一：安装 Rust 版本的 Shadowsocks（推荐）
在 Alpine Linux v3.20 的社区仓库中，有一个更现代、性能更好的 Rust 版本 `shadowsocks-rust`，你可以直接安装它：

1. **安装 `shadowsocks-rust`**
```bash
apk update
apk add shadowsocks-rust
```

2. **创建配置文件**
```bash
mkdir -p /etc/shadowsocks
cat > /etc/shadowsocks/config.json << 'EOF'
{
    "server": "0.0.0.0",
    "server_port": 8388,
    "password": "123456",
    "method": "aes-256-gcm",
    "mode": "tcp_and_udp"
}
EOF
```
请将 `123456` 替换为你自己的密码。

### 正确启动方式

#### 方式一：前台运行（适合测试）
直接运行，会占用当前终端：
```bash
ssserver -c /etc/shadowsocks/config.json
```

#### 方式二：后台运行（推荐）
使用 `nohup` 或 `&` 让服务在后台运行：
```bash
# 使用 & 放到后台
ssserver -c /etc/shadowsocks/config.json &

# 或者使用 nohup，即使关闭终端也继续运行
nohup ssserver -c /etc/shadowsocks/config.json > /var/log/shadowsocks.log 2>&1 &
```

#### 方式三：使用 OpenRC 服务
Alpine 提供了官方的 OpenRC 服务脚本，你可以这样管理：

1. 启动服务：
```bash
rc-service shadowsocks-rust start
```

2. 设置开机自启：
```bash
rc-update add shadowsocks-rust default
```

3. 查看服务状态：
```bash
rc-service shadowsocks-rust status
```