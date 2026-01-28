# 1. 来自 tyooe.com 的 Linux 服务器性能/网络测试脚本合集(转载)
> 互联网创作，仅供个人使用，如有侵权，请联系删除
**来源**：[https://tyooe.com/linux-benchmark-scripts](https://tyooe.com/linux-benchmark-scripts)

**用途**：适用于新购 Linux 服务器后，快速评估硬件性能、网络连通性、流媒体解锁状态等，包含综合测试、专项性能测试、网络测试、解锁检测四大类脚本，可直接复制命令执行。

## 2. 综合测试脚本
### 2.1 YABS (Yet Another Bench Script)
服务器行业常用综合测试脚本，自动执行磁盘（fio）、网络（iperf3）、CPU/内存（Geekbench）基准测试。

使用（任意一条命令，复制即可执行）：
```bash
curl -sL yabs.sh | bash
```
```bash
wget -qO- yabs.sh | bash
```
地址：https://github.com/masonr/yet-another-bench-script

### 2.2 VPS 融合怪服务器测评脚本
聚合多款脚本的 all-in-one 工具，含 IP 质量、流媒体解锁、大陆网络路由测试，无 Geekbench 测试。

使用（任意一条命令，复制即可执行）：
```bash
curl -L https://github.com/spiritLHLS/ecs/raw/main/ecs.sh -o ecs.sh && chmod +x ecs.sh && bash ecs.sh
```
```bash
bash <(wget -qO- bash.spiritlhl.net/ecs)
```
地址：https://github.com/spiritLHLS/ecs

### 2.3 LemonBench
快速评估服务器综合性能，提供硬件配置信息的专用工具。

使用（任意一条命令，复制即可执行）：
```bash
wget -qO- https://raw.githubusercontent.com/LemonBench/LemonBench/main/LemonBench.sh | bash -s -- --fast
```
```bash
curl -fsL https://raw.githubusercontent.com/LemonBench/LemonBench/main/LemonBench.sh | bash -s -- --fast
```
地址：https://github.com/LemonBench/LemonBench

### 2.4 Benchy
YABS 克隆修改版，输出结果更精简。

使用（任意一条命令，复制即可执行）：
```bash
wget -qO- benchy.pw | sh
```
```bash
curl -Ls benchy.pw | sh
```
地址：https://github.com/L1so/benchy

### 2.5 Bench.Monster
包含系统信息查询、I/O 测试、全球节点速度测试的 VPS/服务器测速工具。

使用（复制即可执行）：
```bash
curl -sL bench.monster | bash
```
地址：https://bench.monster/

### 2.6 Bench.sh
秋水逸冰开源，自动测试 I/O 性能和上传下载速度。

使用（任意一条命令，复制即可执行）：
```bash
wget -qO- bench.sh | bash
```
```bash
curl -Lso- bench.sh | bash
```
地址：https://github.com/teddysun/across

### 2.7 Superbench.sh
基于 oooldking 脚本修改，新增 Geekbench、流媒体及大陆三网回程路由测试。

使用（复制即可执行）：
```bash
bash <(wget -qO- https://down.vpsaff.net/linux/speedtest/superbench.sh)
```
地址：https://www.idcoffer.com/archives/4764

## 3. 性能测试
### 3.1 Unixbench.sh
秋水逸冰开源，类 Unix 系统性能测试工具，涵盖系统调用、读写、进程、运算等多项基准测试，运行 10-30 分钟，分数越高性能越好。

使用（分步复制执行）：
```bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/unixbench.sh
```
```bash
chmod +x unixbench.sh
```
```bash
./unixbench.sh
```
地址：https://github.com/teddysun/across

### 3.2 i-abc / GB5
专用于服务器的 Geekbench 5 测试工具，专注 CPU 单核/多核性能评分。

使用（任意一条命令，复制即可执行）：
```bash
bash <(curl -sL bash.icu/gb5)
```
```bash
bash <(wget -qO- https://raw.githubusercontent.com/i-abc/GB5/main/gb5-test.sh)
```
地址：https://github.com/i-abc/gb5

### 3.3 memoryCheck
一键检测 VPS 内存是否超售（含 Swap、气球驱动、KSM 内存合并），支持一键关闭冗余选项提升性能。

使用（任意一条命令，复制即可执行）：
```bash
curl https://raw.githubusercontent.com/uselibrary/memoryCheck/main/memoryCheck.sh | bash
```
```bash
wget --no-check-certificate -O memoryCheck.sh https://raw.githubusercontent.com/uselibrary/memoryCheck/main/memoryCheck.sh && chmod +x memoryCheck.sh && bash memoryCheck.sh
```
关闭气球驱动（测试后执行）：
```bash
rmmod virtio_balloon
```
地址：https://github.com/uselibrary/memoryCheck

### 3.4 硬盘性能/通电时间检测（独立服务器）
独立服务器专用，测试硬盘信息、性能及通电时间；VPS 仅显示基本信息。

使用（任意一条命令，复制即可执行）：
```bash
bash <(wget -qO- git.io/ceshi)
```
```bash
bash <(curl -Ls git.io/ceshi)
```
```bash
bash -c "$(wget -qO- https://github.com/Aniverse/A/raw/i/a)"
```
```bash
wget -q https://github.com/Aniverse/A/raw/i/a && bash a
```
地址：https://github.com/Aniverse/A

## 4. 网络测试
### 4.1 网速测试
#### 4.1.1 network-speed.xyz
基于 Ookla speedtest.net 服务，支持全球/区域测速，展示网速、延迟、数据包丢失，完整测试耗流量较大（约 90G），限流量服务器建议选区域测试。

使用（任意一条命令，复制即可执行）：
```bash
curl -sL network-speed.xyz | bash
```
```bash
wget -qO- network-speed.xyz | bash
```
地址：https://network-speed.xyz/

#### 4.1.2 i-abc / Speedtest
多功能自更新测速脚本，支持大陆三网+教育网 IPv4 多线程测速。

使用（任意一条命令，复制即可执行）：
```bash
bash <(curl -sL bash.icu/speedtest)
```
```bash
bash <(curl -sL https://raw.githubusercontent.com/i-abc/Speedtest/main/speedtest.sh)
```
地址：https://github.com/i-abc/speedtest

#### 4.1.3 HyperSpeed
专注中国大陆电信、联通、移动节点的测速工具。

使用（复制即可执行）：
```bash
bash <(wget -qO- https://bench.im/hyperspeed)
```
地址：https://github.com/veoco/bim-core/tree/main

### 4.2 路由测试
#### 4.2.1 三网回程路由测试
测试中国大陆电信、联通、移动回程路由状态。

使用（复制即可执行）：
```bash
curl https://raw.githubusercontent.com/zhanghanyun/backtrace/main/install.sh -sSf | sh
```
地址：https://github.com/zhanghanyun/backtrace

#### 4.2.2 mtr_trace
检测 VPS 回程国内三网路由的专用脚本。

使用（复制即可执行）：
```bash
curl https://raw.githubusercontent.com/zhucaidan/mtr_trace/main/mtr_trace.sh | bash
```
地址：https://github.com/zhucaidan/mtr_trace

#### 4.2.3 AutoTrace
测试本机网络信息、IPv4/IPv6 三网回程 TCP 路由，支持自定义 IP 路由测试。

使用（分步复制执行）：
```bash
wget -N --no-check-certificate https://raw.githubusercontent.com/Chennhaoo/Shell_Bash/master/AutoTrace.sh
```
```bash
chmod +x AutoTrace.sh
```
```bash
bash AutoTrace.sh
```
地址：https://github.com/Chennhaoo/Shell_Bash

#### 4.2.4 BestTrace 回程测试
输出 BestTrace 原生回程路由结果，支持多地电信/联通节点测试。

使用（复制即可执行）：
```bash
wget -qO- git.io/besttrace | bash
```
地址：https://github.com/zq/shell

## 5. 流媒体服务/应用解锁测试
### 5.1 RegionRestrictionCheck
测试各类流媒体平台（Netflix、Disney+ 等）及游戏的区域限制解锁情况。

使用（复制即可执行）：
```bash
bash <(curl -L -s check.unlock.media)
```
地址：https://github.com/lmc999/RegionRestrictionCheck

### 5.2 MediaUnlockTest
更快的流媒体检测工具，支持按区域分类检测（跨国/港台/日本等平台）。

使用（复制即可执行）：
```bash
bash <(curl -Ls unlock.moe)
```
地址：https://github.com/nkeonkeo/MediaUnlockTest

### 5.3 NETFLIX-VERIFY
专门检测 IPv4/IPv6 网络对 Netflix 的解锁情况（区分自制剧/非自制剧）。

使用（分步复制执行）：
```bash
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/2.01/nf_2.01_linux_amd64
```
```bash
chmod +x nf
```
```bash
clear && ./nf
```
地址：https://github.com/sjlleo/netflix-verify

### 5.4 TikTokCheck
检测服务器 IP 对应的 TikTok 区域解锁状态。

使用（复制即可执行）：
```bash
bash <(curl -s https://raw.githubusercontent.com/lmc999/TikTokCheck/main/tiktok.sh)
```
地址：https://github.com/lmc999/TikTokCheck

### 5.5 OpenAI-Checker
检测服务器 IP 是否支持访问 ChatGPT（区分 IPv4/IPv6）。

使用（复制即可执行）：
```bash
bash <(curl -Ls https://cdn.jsdelivr.net/gh/missuo/OpenAI-Checker/openai.sh)
```
地址：https://github.com/missuo/OpenAI-Checker

## 6. 注意事项
1. 进行 Geekbench 测试需服务器至少 1G 可用内存，内存不足请先开启 SWAP；
2. Geekbench 测试约耗时 10 分钟，Unixbench 耗时更长（10-30 分钟）；
3. 部分脚本会输出服务器 IP，复制/截屏分享时请模糊化处理。