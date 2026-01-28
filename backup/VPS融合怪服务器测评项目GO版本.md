> 互联网创作，仅供个人使用，如有侵权，请联系删除
融合怪测评项目 - GO版本
开源项目地址:[https://github.com/oneclickvirt/ecs](https://github.com/oneclickvirt/ecs)

---
## **适配系统和架构**

### **编译与测试支持情况**
| 编译支持的架构             | 测试支持的架构 | 编译支持的系统             | 测试支持的系统 |
|---------------------------|--------------|---------------------------|---------------|
| amd64                     | amd64        | Linux                     | Linux         |
| arm64                     | arm64        | Windows                   | Windows       |
| arm                       |              | MacOS(Darwin)             | MacOS         |
| 386                       |              | FreeBSD                   |               |
| mips,mipsle               |              | Android                   |               |
| mips64,mips64le           |              |                           |               | 
| ppc64,ppc64le             |              |                           |               |
| s390x                     | s390x        |                           |               |
| riscv64                   |              |                           |               |

> 更多架构与系统请自行测试或编译，如有问题请开 issues。

### **待支持的系统**

| 系统           | 说明                       |
|----------------|---------------------------|
| OpenBSD/NetBSD | 部分Goalng的官方库未支持本系统(尤其是net相关项目)  |

---

## **功能**

- 系统基础信息查询，IP基础信息并发查询：[basics](https://github.com/oneclickvirt/basics)、[gostun](https://github.com/oneclickvirt/gostun)
- CPU 测试：[cputest](https://github.com/oneclickvirt/cputest)，支持 sysbench(lua/golang版本)、geekbench、winsat
- 内存测试：[memorytest](https://github.com/oneclickvirt/memorytest)，支持 sysbench、dd、winsat、mbw、stream
- 硬盘测试：[disktest](https://github.com/oneclickvirt/disktest)，支持 dd、fio、winsat
- 流媒体平台解锁测试并发查询：[UnlockTests](https://github.com/oneclickvirt/UnlockTests)，逻辑借鉴 [RegionRestrictionCheck](https://github.com/lmc999/RegionRestrictionCheck) 等
- IP 质量/安全信息并发查询：二进制文件编译至 [securityCheck](https://github.com/oneclickvirt/securityCheck)
- 邮件端口测试：[portchecker](https://github.com/oneclickvirt/portchecker)
- 上游及回程路由线路检测：借鉴 [zhanghanyun/backtrace](https://github.com/zhanghanyun/backtrace)，二次开发至 [oneclickvirt/backtrace](https://github.com/oneclickvirt/backtrace)
- 三网路由测试：基于 [NTrace-core](https://github.com/nxtrace/NTrace-core)，二次开发至 [nt3](https://github.com/oneclickvirt/nt3)
- 网速测试：基于 [speedtest.net](https://github.com/spiritLHLS/speedtest.net-CN-ID) 和 [speedtest.cn](https://github.com/spiritLHLS/speedtest.cn-CN-ID) 数据，开发 [oneclickvirt/speedtest](https://github.com/oneclickvirt/speedtest)，同时融合私有国内测速节点
- 三网 Ping 值测试：借鉴 [ecsspeed](https://github.com/spiritLHLS/ecsspeed)，二次开发至 [pingtest](https://github.com/oneclickvirt/pingtest)
- 支持root或admin环境下测试，支持非root或非admin环境下测试，支持离线环境下进行测试，**暂未**支持无DNS的在线环境下进行测试

**本项目初次使用建议查看说明：[跳转](https://github.com/oneclickvirt/ecs/blob/master/README_NEW_USER.md)**

---

## **使用说明**

### **Linux/FreeBSD/MacOS**

#### **一键命令**

**一键命令**将默认**不安装依赖**，默认**不更新包管理器**，默认**非互动模式**

- **国际用户无加速：**

  ```bash
  export noninteractive=true && curl -L https://raw.githubusercontent.com/oneclickvirt/ecs/master/goecs.sh -o goecs.sh && chmod +x goecs.sh && ./goecs.sh install && goecs
  ```

- **国际/国内使用 CDN 加速：**

  ```bash
  export noninteractive=true && curl -L https://cdn.spiritlhl.net/https://raw.githubusercontent.com/oneclickvirt/ecs/master/goecs.sh -o goecs.sh && chmod +x goecs.sh && ./goecs.sh install && goecs
  ```

- **国内用户使用 CNB 加速：**

  ```bash
  export noninteractive=true && curl -L https://cnb.cool/oneclickvirt/ecs/-/git/raw/main/goecs.sh -o goecs.sh && chmod +x goecs.sh && ./goecs.sh install && goecs
  ```

- **短链接：**

  ```bash
  export noninteractive=true && curl -L https://bash.spiritlhl.net/goecs -o goecs.sh && chmod +x goecs.sh && ./goecs.sh install && goecs
  ```

  或

  ```bash
  export noninteractive=true && curl -L https://ba.sh/JrVa -o goecs.sh && chmod +x goecs.sh && ./goecs.sh install && goecs
  ```
