## 第一步：创建管理脚本

```bash
# 创建脚本文件
nano ~/bot-manager.sh
```

## 第二步：复制粘贴内容

将以下内容完整复制粘贴到 nano 编辑器中：

```bash
#!/bin/bash

case "$1" in
    start)
        echo "启动机器人服务..."
        screen -dmS napcat bash -c 'proot-distro login napcat -- bash -c "xvfb-run -a qq --no-sandbox"'
        screen -dmS astrobot bash -c 'proot-distro login ubuntu -- bash -c "cd AstrBot && uv run main.py"'
        echo "启动完成！"
        ;;
    stop)
        echo "停止机器人服务..."
        screen -S napcat -X quit 2>/dev/null
        screen -S astrobot -X quit 2>/dev/null
        echo "已停止"
        ;;
    restart)
        $0 stop
        sleep 3
        $0 start
        ;;
    status)
        echo "当前运行状态："
        screen -ls
        ;;
    napcat)
        screen -r napcat
        ;;
    astrobot)
        screen -r astrobot
        ;;
    *)
        echo "使用方法: $0 {start|stop|restart|status|napcat|astrobot}"
        echo "  start    - 启动所有服务"
        echo "  stop     - 停止所有服务"
        echo "  restart  - 重启所有服务"
        echo "  status   - 查看状态"
        echo "  napcat   - 连接到 NapCat"
        echo "  astrobot - 连接到 AstrBot"
        exit 1
        ;;
esac
```

## 第三步：保存文件

按顺序按键：

```
Ctrl + X → Y → Enter
```

## 第四步：给脚本添加执行权限

```bash
chmod +x ~/bot-manager.sh
```

## 第五步：使用脚本管理机器人

启动所有服务：

```bash
./bot-manager.sh start
```

停止所有服务：

```bash
./bot-manager.sh stop
```

查看运行状态：

```bash
./bot-manager.sh status
```

重启所有服务：

```bash
./bot-manager.sh restart
```

连接到 NapCat 控制台：

```bash
./bot-manager.sh napcat
```

（要退出控制台，按 Ctrl + A 然后按 D）

连接到 AstrBot 控制台：

```bash
./bot-manager.sh astrobot
```

（要退出控制台，按 Ctrl + A 然后按 D）

查看帮助信息：

```bash
./bot-manager.sh
```

或者

```bash
./bot-manager.sh help
```

## 使用示例流程：

1. 第一次启动：
   ```bash
   ./bot-manager.sh start
   ```
2. 检查是否正常运行：
   ```bash
   ./bot-manager.sh status
   ```
3. 查看 NapCat 日志：
   ```bash
   ./bot-manager.sh napcat
   # 看完后按 Ctrl+A, 然后按 D 退出
   ```
4. 停止服务：
   ```bash
   ./bot-manager.sh stop
   ```
## 结束提示
这样就只需要记住一个脚本文件和几个简单的命令参数，非常方便管理！

在linux一件脚本下的启动命令
1. 基础持续启动（指定QQ）：输入命令后，程序后台持续运行，断开终端也不受影响
```
napcat start 你的QQ号
```

​
2. 配置开机自启（彻底无忧，长期运行必配）：配置后服务器重启也会自动启动NapCatQQ
```
napcat startup 你的QQ号
```
